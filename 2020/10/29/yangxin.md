# A Trip Down the DNS Rabbit Hole: Understanding the Role of Kubernetes, Golang, libc, systemd
介绍了一下事情的起因，想引入一个新的工具，在本地测试的时候出现了issue，发现是dns的锅
## DNS resolution with libc, Golang, systemd

 > How DNS queries are performed depends on a combination of application code, GNU libc configuration, and operating system configuration.

### not  on systemd operating systems
- libc解析dns的配置写在/etc/nsswitch.conf
```
hosts:          files dns
```
- 如果/etc/host里没有配置域名，就会继续向网络中的DNS服务器进行dns查询

###  on systemd operating systems
- 查看/etc/nsswitch.conf
```
hosts:      files dns myhostname
```
- myhostname是一个nss(Name Service Switch)插件
  - 本地配置的hostName解析成本地环回地址  127.0.0.1  ::1
  - "localhost" and "localhost.localdomain" (as well as any hostname ending in ".localhost" or ".localhost.localdomain")解析成本地环回地址  127.0.0.1  ::1
  - The hostname "_gateway" is resolved to all current default routing gateway addresses, ordered by their metric. 
  
 ### on golang program
 - golang不使用libc，对resolver的选择发生在runtime
    - 默认是使用pure go resolver，因为一次go的dns阻塞调用只会消费一个goroutine ，, while a blocked C call consumes an operating system thread. 
    - the cgo-based resolver
      - on systems that do not let programs make direct DNS requests (OS X)
      - when the LOCALDOMAIN environment variable is present (even if empty)
      - when the RES_OPTIONS or HOSTALIASES environment variable is non-empty
      - when the ASR_CONFIG environment variable is non-empty (OpenBSD only)
      - when /etc/resolv.conf or /etc/nsswitch.conf specify the use of features that the Go resolver does not implement
      - when the name being looked up ends in .local or is an mDNS name
      
- Kuberhealthy 使用pure go resolver，which use /etc/resolv.conf to determine where and how to send the DNS query

# 总结
我看晕了，感觉这篇太绕了
## coredns and minikube DNS query routing
```
container$ cat /etc/resolv.conf
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local
options ndots:5
```
- Kubernetes的启动项--cluster-dns和--cluster-domain会写入这些配置项
- 搜索kube-health.localhost，由于少于5个点，会加上后缀搜索
- 10.96.0.10是一个cluster-ip，round-robined to the Pod ip addresses that are part of the kube-dns
- corednsplugin会相应所有域名以cluster.local结尾的query
- corednsplugin没有该dns记录，fall through to plugin插件
- default behavior of Docker is to use the /etc/resolve.conf，DNS queries for non-Kubernetes services from inside Pods will be routed
```
minikube$ cat /etc/resolv.conf

nameserver 10.0.2.3
```
- 10.0.2.3 is the nameserver that VirtualBox creates for NAT interfaces.
- 所以什么都没找到

- 在centos可以成功，因为centos用systemd
- This causes a libc resolve of any hostname ending in ".localhost" or ".localhost.localdomain") are resolved to the IP addresses 127.0.0.1 and ::1.
