---
layout: default
title:  "Distance Vector Routing Protocol"
date:   2015-12-13 02:54:25 +0500
categories: project
description: Implementation of distance-vector routing protocol using Bellman-Ford algorithm in Python. It uses Google's Protocol Buffers for packaging the data to be communicated amongst the routers.

---
# **Distance Vector Routing Protocol**
## **Overview**
**Semester:** 4th

**Project Date:** May 2017

**Project Duration:** ~4 staggered days

**Languages/Frameworks:** Python, Protocol Buffers

**Github Page:** [Link](https://github.com/haseebs/Distance-vector-routing-protocol)

**Short Description:** Implementation of distance-vector routing protocol using Bellman-Ford algorithm in Python. It uses Google's Protocol Buffers for packaging the data to be communicated amongst the routers in a simulated network.

## **Prerequisites**
* Python
* Protocol Buffers
* Tmux and Tmuxinator [Both Optional, for starting multiple routers at once]

## **Usage**
For starting multiple routers at once (Recommended):
* Modify the [root] attribute in nodes.yml to point to where the project was cloned
* Place the nodes.yml in ~/.tmuxinator/nodes.yml
* cd to root of the project folder
* Execute ```tmuxinator start nodes``` 

For starting a router manually:
```bash
python DVR.py [router-id] [any-unique-port-no] [path-to-router-config-file]
```

## **Description of the Process**
Upon  initialization, each router creates a distance vector update packet and sends this packet to all direct neighbors. Upon  receiving  this distance vector update packet, each  neighbour incorporates the information into its  routing table. Each router  periodically broadcasts the distance vector update packet to its neighbors every set number of seconds.

Upon recieving the distance vector update packet from the neighbours, each router builds up a reachability matrix. Using this matrix, each router can execute Bellman-Ford algorithm to calculate the least-cost paths to its neighbouring routers. Whenever a path or the cost of a path is changed, distance vector update packet is created again and sent to all the neighbouring routers. This processes keeps going until all the routers converge to least-cost paths.

If at any time during or after this process, a link cost is changed or a path is made unavailable, the change is accomodated as the change is noticed by some router due to timeout (in case it is made unavailable) and notified to all the neighbours through updates until the entire network converges again.

## **Implementation Details**
### **Config Files**
The topology is built using router data from the configfiles. Each router has a configfile which contains the data in format:
```
number_of_neighbours
neighbour_1_cost neighbour_1_port
neighbour_2_cost neighbour_2_port
```
Where the port is used by the neighbour to recieve the distance vector update packets over a UDP connection.

### **Distance Vector update format**
Protocol Buffers are language-independent mechanism for serializing
structured messages into binary format. They are smaller and more
efficient than other serialization methods. However, they require a bit
of setup before they can be used.

All the packets are serialized using Protocol Buffers at the sender side
and deserialized at the reciever's end. The format for the message is
given in distanceVec.proto file which is then compiled into a python library.

Each update packet contains the information in format as:
```
String source_id
Array Neighbours
```
Where each Neighbour is defined as:
```
string neighbour_id
float cost
```

### **Threads**
For each instance of execution, following threads are launched:
* SendDV for sending distance vector packets
* bford for Bellman Ford algorithm
* Server for receiving distance vector packets
* IO for displaying and changing values in reachability matrix
* TimeOut for detecting nodes that are down

### **Sending and Receiving of Distance Vectors**
Regular distance vector packets are sent every 10 seconds. If there is any
change in the distance vector of a node, it does not wait for this timer,
a distance vector packet is sent immediately to new nodes in its neighborhood.

The sender implements the Poison Reverse to eliminate count to infinity.
The infinity value is taken as 16. This limits the network diameter to cost
of 15 around the node.

### **Link Cost Changes**
Link costs can be viewed in tabular format by entering 0 and changed by entering
1 anytime during the program’s execution. When a link cost is changed, a
triggered update is sent to all neighbors notifying them of this change if
this change resulted in a change in the current node’s distance vector values.

### **Node Failure**
Failed nodes are detected in the TimeOut thread. The time put period is set in
TIMEOUT_PERIOD constant, which is 30 seconds.  This timer is initialized for all
nodes as soon as the server for the current node starts.

TimeOut thread maintains a list of timers, one for each neighbor. When a packet
is received from a neighbor, its timer is reset. If a timer belonging to a
neighbor hits the TIMEOUT_PERIOD, all the values to or through that node in
the reachability matrix are set to MAX_NETWORK_SIZE (16) marking that node as
unreachable and an updated distance vector packet is sent to all neighbors.

### **Bellman-Ford**
Bellman Ford is executed when there is a change in the reachability matrix.

When a packet is received, if the neighbor was not marked as unreachable
previously, it fills the reachability matrix as it normally would and
checks if there is any change in this matrix. If there is, a new distance
vector packet is constructed. If this distance vector packet is different from
the previous one, the 10 second timer is ignored and a triggered update
is sent to all neighbors.

If upon receiving a packet, the origin of the packet was marked as unreachable
(which indicates that the node was down but now has been restarted), the cost
to that neighbor directly is updated by checking its distance vector for value
pointing towards the current node. All the subsequent values to or through that
node are then updated by using this value instead of MAX_NETWORK_SIZE, which was
the cost marked towards that node. This will always result in a change in the
distance vector and thus a triggered update is sent to all neighbors.
