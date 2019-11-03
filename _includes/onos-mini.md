### Adding required modules in ONOS
In order to connect mininet to ONOS, we need to add
two applications in ONOS list of applications


```
ubuntu@root > app activate org.onosproject.fwd
ubuntu@root > app activate org.onosproject.openflow
ubuntu@root > apps -a -s                                                10:51:19
*   2 org.onosproject.drivers              2.3.0.SNAPSHOT Default Drivers
*  29 org.onosproject.intentsynchronizer   2.3.0.SNAPSHOT Intent Synchronizer
*  45 org.onosproject.lldpprovider         2.3.0.SNAPSHOT LLDP Link Provider
*  53 org.onosproject.hostprovider         2.3.0.SNAPSHOT Host Location Provider
*  54 org.onosproject.optical-model        2.3.0.SNAPSHOT Optical Network Model
*  55 org.onosproject.openflow-base        2.3.0.SNAPSHOT OpenFlow Base Provider
*  56 org.onosproject.openflow             2.3.0.SNAPSHOT OpenFlow Provider Suite
* 101 org.onosproject.gui2                 2.3.0.SNAPSHOT ONOS GUI2
* 133 org.onosproject.vpls                 2.3.0.SNAPSHOT VLAN L2 Broadcast Network
* 166 org.onosproject.fwd                  2.3.0.SNAPSHOT Reactive Forwarding
```

From the mininet terminal run the command below to check
connectivity. If the configuration is correct, mininet hosts
will be able to send traffic to each other
```
$ sudo mn --controller remote,ip=127.0.0.1
mininet> pingall
*** Ping: testing ping reachability
h1 -> h2 
h2 -> h1 
*** Results: 0% dropped (2/2 received)
```

### Check added flows on ONOS CLI
```
ubuntu@root > flows                                                     10:55:27
deviceId=of:0000000000000001, flowRuleCount=4
    id=100007a585b6f, state=ADDED, bytes=0, packets=0, duration=370, liveType=UNKNOWN, priority=40000, tableId=0, appId=org.onosproject.core, selector=[ETH_TYPE:bddp], treatment=DefaultTrafficTreatment{immediate=[OUTPUT:CONTROLLER], deferred=[], transition=None, meter=[], cleared=true, StatTrigger=null, metadata=null}
    id=100009465555a, state=ADDED, bytes=0, packets=0, duration=370, liveType=UNKNOWN, priority=40000, tableId=0, appId=org.onosproject.core, selector=[ETH_TYPE:lldp], treatment=DefaultTrafficTreatment{immediate=[OUTPUT:CONTROLLER], deferred=[], transition=None, meter=[], cleared=true, StatTrigger=null, metadata=null}
    id=10000ea6f4b8e, state=ADDED, bytes=168, packets=4, duration=370, liveType=UNKNOWN, priority=40000, tableId=0, appId=org.onosproject.core, selector=[ETH_TYPE:arp], treatment=DefaultTrafficTreatment{immediate=[OUTPUT:CONTROLLER], deferred=[], transition=None, meter=[], cleared=true, StatTrigger=null, metadata=null}
    id=10000021b41dc, state=ADDED, bytes=196, packets=2, duration=370, liveType=UNKNOWN, priority=5, tableId=0, appId=org.onosproject.core, selector=[ETH_TYPE:ipv4], treatment=DefaultTrafficTreatment{immediate=[OUTPUT:CONTROLLER], deferred=[], transition=None, meter=[], cleared=true, StatTrigger=null, metadata=null}
```

