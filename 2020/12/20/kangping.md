# Linux CPU load

## CPU load
Understand the load measure of Unix/Linux and have a better sense when machine load is high

## Background
On a single processor machie, load could be above 1 when scheduling has backups, which typically means you need to lower the workload on the machine. Load == 1 is not necessarily the best situation either, since it leaves no headroom for the system. In practice, we set the warning line at load==0.7. (This is when things like Nagios start to spam your inbox.)

For multi-processor machine, we just multiply values by number of processors. 

But `uptime` returns three values. They are the 1-minute, 5-minute and 15-minute average load of the system. Typically the longer horizon average is more important when your system is not latency sensitive. (There are plenty cases where your latency requirement is sub-minute. In cases like that, you probably want to focus more on shorter term load, have a lower warning threshold or even better not relying on `uptime` )

