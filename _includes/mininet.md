### Mininet Installation  and basic testing
```
$ git clone git://github.com/mininet/mininet
$ cd mininet
$ git tag # list available versions
$ git checkout -b 2.2.1 2.2.1 # or whatever version you wish to install
$ cd ..
$ mininet/util/install.sh
```
Check mininet options
```
$ sudo mn -h
```
Start mininet minimal topology
```
$ sudo mn -h
```
Display nodes
```
mininet> nodes
```
Display Links
```
mininet> net
```
Dump information about nodes
```
mininet> dump
```

### Mininet Networking
Check the network information of mininet host and switches
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

Check processes running on the host

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

### Test connectivity between hosts
Now, verify that you can ping from host 0 to host 1:
```
mininet> h1 ping -c 1 h2
```
If a string appears later in the command with a node name, that node name is replaced by 
its IP address; this happened for h2.

You should see OpenFlow control traffic. The first host *ARPs* for the *MAC* address of the 
second, which causes a *packet_in* message to go to the controller. The controller then sends 
a *packet_out* message to flood the broadcast packet to other ports on the switch (in this 
example, the only other data port).

The second host sees the ARP request and sends a reply. This reply goes to the controller, which
 sends it to the first host and pushes down a flow entry. Now the first host knows the MAC 
address of the second, and can send its ping via an ICMP Echo Request. This request, along 
with its corresponding reply from the second host, both go the controller and result in a 
flow entry pushed down (along with the actual packets getting sent out).

Repeat the last ping:
```
mininet> h1 ping -c 1 h2
```
You should see a much lower ping time for the second try (< 100us). A flow entry covering ICMP 
ping traffic was previously installed in the switch, so no control traffic was generated, and 
the packets immediately pass through the switch. An easier way to run this test is to use the 
Mininet CLI built-in pingall command, which does an all-pairs ping:
```
mininet> pingall
```
### Running simple web server and client
Try starting a simple HTTP server on h1, making a request from h2, then shutting down 
the web server:
```
mininet> h1 python -m SimpleHTTPServer 80 & 
mininet> h2 wget -O - h1 
mininet> h1 kill %python
```
### Mininet Cleanup
Exit the CLI
```
mininet> exit
```
If Mininet crashes for some reason, clean it up:
```
$ sudo mn -c
```
### Advanced Options with mininet and OpenFlow switch
Run a regression test
```
$ sudo mn --test pingpair
```
Another useful test is iperf (give it about 10 seconds to complete,
```
$ sudo mn --test iperf
```
### Changing topology size
The default topology is a single switch connected to two hosts. You 
could change this to a different topo with --topo, and pass parameters 
for that topologys creation.

Verify all-pairs ping connectivity with one switch and three hosts:

Run a regression test:
```
$ sudo mn --test pingall --topo single,3
``` 
Kill the mininet process and clear network for next part
