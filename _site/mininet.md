### Mininet Installation  and basic testing
```
$ git clone git://github.com/mininet/mininet
$ cd mininet
$ git tag # list available versions
$ git checkout -b 2.2.1 2.2.1 # or whatever version you wish to install
$ cd ..
$ mininet/util/install.sh
```

1. Check mininet options
```
$ sudo mn -h
```
2. Start mininet minimal topology
```
$ sudo mn -h
```
3. Display nodes
```
mininet> nodes
```
4. Display Links
```
mininet> net
```
5. Dump information about nodes
```
mininet> dump
```

### Mininet Networking
1. Check the network information of mininet host and switches
If the first string typed into the Mininet CLI is a host, switch or 
controller name, the command is executed on that node. Run a command 
on a host process:
```
mininet> h1 ifconfig -a
``` 
You should see the hosts h1-eth0 and loopback (lo) interfaces. Note that 
this interface (h1-eth0) is not seen by the primary Linux system when 
**ifconfig** is run, because it is specific to the network namespace of 
the host process. In contrast, the switch by default runs in the root network 
namespace, so running a command on the switch is the same as running it from 
a regular terminal:
```
mininet> s1 ifconfig -a
```
This will show the switch interfaces, plus the VMs connection out (eth0).

2. Check processes running on the host

Note that only the network is virtualized; each host process sees the same set 
of processes and directories. For example, print the process list from a host process:
```
mininet> h1 ps -a
```
This should be the exact same as that seen by the root network namespace:
```
mininet> s1 ps -a
```
It would be possible to use separate process spaces with Linux containers, but currently 
Mininet doesnt do that. Having everything run in the root process namespace is convenient 
for debugging, because it allows you to see all of the processes from the console using 
ps, kill, etc.

