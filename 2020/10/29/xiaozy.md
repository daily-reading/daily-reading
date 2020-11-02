> 原文地址 https://medium.com/box-tech-blog/a-trip-down-the-dns-rabbit-hole-understanding-the-role-of-kubernetes-golang-libc-systemd-41fd80ffd679

1. `/etc/nsswtich.conf` 文件定义了是优先看 `/etc/hosts` 还是访问 DNS Server，这里示例中 files dns 表示先看本地文件，即 `/etc/hosts`
    - `myhostname` 是 NSS 插件实现的解析扩展，遵循下列规则
        1. 本地配置的域名解析到配置的 IP，本地配置的域名没配置 IP，则解析到 `127.0.0.1` / `：：1`
        2. `localhost`, `localhost.localdomain` 和以 `localhost` 和 `localhost.localdomain` 结尾的域名解析到 `127.0.0.1`
        3. `_gateway` 解析到所有的默认路由网关
    ```bash
    container$ cat /etc/nsswitch.conf
    # /etc/nsswitch.conf
    ...
    hosts:          files dns myhostname
    ...
    ```

2. `lic` 相关的函数（例如）解析过程按照 `/etc/nsswitch` 中 `hosts` 部分指定的顺序，这里的例子是 `/etc/hosts` -> `/etc/resolve.conf` 中指定的 **DNS Server** -> NSS 插件
3. Go 有自己的解析器 （Resolver），独立于 lic 存在
    1. 可以通过 `CGO_ENABLED` 环境变量来指定是否启用 lic 的解析， `CGO_ENABLED=0` 表示只用 `Go resolver`
