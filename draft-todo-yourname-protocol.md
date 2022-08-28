---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "An Architecture for Space Network Emulation"
category: info

docname: draft-hou-yourname-arch-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

author:
 -
    fullname: Dongxu Hou
    organization: Your Organization Here
    email: your.email@example.com

normative:

informative:


--- abstract

This document describes an emulation architecture which provides a realistic, flexible, and extensible experimental environment for the space network (SN). The architecture includes four components, namely logical plane, control plane, data plane and measurement plane. Software-defined networking (SDN), virtualization technology, and traffic control mechanism are adopted to realize the real space environment and arbitrary topology. Furthermore, an extensible structure is described to emulate large-scale scenarios and access external emulation resources.

--- middle

# Introduction

With the proliferation of low cost spatial information system, powerful on-board processing capacity, and mature inter-satellite link technique, space network (SN) is emerging as an important part of national infrastructure and research frontier. However, due to some unique characteristics of SN, e.g. long propagation delay, frequent link disruption, and etc., mature terrestrial networking technologies cannot be applied to SN directly. To tackle this problem, network researchers propose various new network protocols and technologies. Meanwhile, the process of designing and developing network protocols and technologies is complex, in which experiment phase plays an important role to decipher the performance and make helpful suggestions for improvement. So it is urgent to setup an emulation architecture to carry out the research on basic theory and key technologies for SN.

Typical network experimental validation approaches include simulation, live testbed, and emulation. Discrete-event network simulation has enough flexibility, e.g. ns-2 and OPNET. But the network simulation has no real network traffic, and is hard to realize the real description of complex SN equipment and its environment through the abstraction of various environmental parameters. Actual hardware based testbed has the highest emulation authenticity, e.g. ORBIT, UMass DieselNet. However, for the high cost and poor extendibility of SN nodes, it is difficult for the testbed to support the requirements of changeable SN emulation scenarios.

Compared with network simulation and live testbed, network emulation is a hybrid approach which balances authenticity and flexibility. But owing to the particularity of SN communication environment, existing emulation methods are difficult to balance the authenticity, flexibility and extendibility of the emulation. Typical emulators, e.g. Mininet and vEmulab, only support the emulation of static terrestrial network. [19] proposes TUNIE which is capable of simulating reliable DTN environments and obtaining an accurate system performance evaluation. [20] presents a state-of-the-art DTN testbed for satellite and space communications. The core of the testbed relies on the Bundle Protocol and its architecture has been designed to support multiple DTN implementations and a variety of underlying and overlying protocols. Based on OpenStack, [2] proposes a large-scale, real-time and distributed emulation platform for DTN, namely EmuStack. These platforms are designed for some types of network architecture, such as DTN, and lack support for others. [1] designs a network protocol validation testbed for integrated space-terrestrial network. However, large-scale scenarios emulated on the testbed is realized by the expansion of routing table, which lacks enough authenticity. Overall, in consideration of authenticity, flexibility and extendibility, all the emulators above focus on limited aspects.

This document aims to present an SN emulation architecture which realizes the reliable emulation of dynamic network topology, time-varying link characteristics, multiple protocol architectures, and large-scale network scenarios with the goal of authenticity, flexibility and extendibility. This architecture consists of logical plane, control plane, data plane, and measurement plane. The emulation architecture has a hierarchical star structure to support hardware resource expansion and access external physical devices. Integrating with software defined networking (SDN) and traffic control mechanism, the architecture supports the emulation of dynamic SN. Combining with virtualization technology, various SN protocols and communication technologies could be implemented on the architecture flexibly as real deployments. Considering changeable simulation scenarios and different network concerns, the flexible configuration of network measurement on demand is realized.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# SN Emulation Architecture

Figure 1 describes the SN emulation architecture which can be divided into four planes, namely logical plane, control plane, data plane, and measurement plane. A particular network scenario is set at logical plane. Corresponding settings are stored in database. Control plane reads these settings and procedures command to drive emulation in data plane. Measurement plane is served to monitor network performances of simulation scenarios.

## Logical Plane
According to the specific network scenario, a network model could be built by logical plane which includes space-based backbone transmission network, satellite links, space-based access network, ground stations, terrestrial network, and end-users. The network model contains not only parameters of network nodes, but also specific connection plan, channel parameters, software deployments, and network protocols.

## Control Plane 
Control plane consists of two parts: the front-end service and the back-end service. The front-end service offers aWeb UI to interact with researchers. Researchers can configure various parameters of the network model and visualize the network scenario through the Web UI. The back-end service is backed by the main controller which is responsible for implementing commands and parameters from logical plane into data plane. The main controller is a software and operates all elements in data plane. Researchers can configure and control the environment parameters with different emulation scenarios, as well as monitor the traffic for each testing through control plane. The emulation architecture is thus configurable and controllable.
 
## Data Plane
Data plane is served to emulate network scenarios directly, which contains SDN switches, servers, emulation nodes and etc. These underlying network resources are connected with each other in a hierarchical star network structure. Emulation nodes are connected to the virtual switch on the server and represent SN nodes at logical plane. Diverse software and applications are installed in the emulation node. Servers and external devices or networks are linked to each other by the SDN hardware switch.

## Measurement Plane
Measurement plane mainly has two functions, namely real-time network information collection and full packet capture. The capture container is the basic unit of measurement plane. Each capture container covers a Python-based Daemon which realizes network information collection and supports full packet capture according to the configuration of users. Measurements are stored in database and real-time network information is displayed by the Web UI.

# Physical Implementation Model
Corresponding to the emulation architecture, a typically physical implementation model is presented which has four principal parts: Web UI, main controller, emulation node and SDN switch, as shown in Figure 2.

## Web UI
Users interact with the emulation architecture through the Web UI. The Web UI stores network model parameters configured by users into the database, and show network models in real time to make users observe the emulation process intuitively.

## Main Controller
The main controller interacts with the Web UI and is served for parameter calculation, SDN switch control and node management. The calculation results are written to the database for subsequent emulation operations. The parameter calculation refers to the process that the main controller reads the network model parameters, obtains the relationships of visibility and link characteristics between nodes, filters the connection relationships, and assigns the network configuration of emulation nodes. The SDN switch control refers to the process that the main controller sends topology control flow tables to SDN switches in real time according to the target network topology. The node management refers to the process that the main controller creates emulation nodes, binds nodes with SDN switches, and allocates emulation resources. Besides, the main controller is also responsible for generating the experimental start time and transmitting the operation state of the experiment to the Web UI.

## Emulation Node
All emulation nodes run real network protocol stacks and applications to complete network traffic exchange. Due to the advantages of flexible configuration, easy extension and convenient management, virtual nodes are generally used to simulate the nodes in the target network. For some special needs or cases where emulation can not be supported by pure virtual nodes, physical nodes are used, including embedded devices, satellite links, and etc.

## SDN Switch
According to assigned network parameters, each emulation node is linked to corresponding SDN switch by the main controller. SDN software switch is responsible for the connection of virtual nodes. Meanwhile SDN hardware switch is in charge of the connection of physical nodes, and provides access to external networks or links for the emulation architecture. SDN software switch and SDN hardware switch are connected with each other to form an interconnected emulation network. Once the emulation experiment starts, the main controller sends real-time flow tables to each SDN switch to simulate the dynamic topology of the target network model. SDN switches can also collect and send their own states back to the Web UI for displaying, or save these states for subsequent analysis.

# Emulation Node Design
Emulation node is actual carrier of network protocols and communication technologies which is the basic emulation element of the emulation architecture.

## Node Virtualization
Virtualization is a resource management technology, which realizes the goal of virtualizing a physical computer system into multiple virtual computer systems. Typical virtualization methods include virtual machine and container. This document takes the container-based virtualization, such as Docker, as an example to illustrate the emulation node design. Moreover, container orchestration, such as Kubernetes, could be employed to achieves functions that cannot be supported by native container-based virtualization. A Pod, which is the basic arrangement unit, usually covers multiple containers in Kubernetes. Containers in a Pod share some resources with each other, e.g. storage and network resources.

Figure 3 describes an emulation node which is built by a Pod consists of an emulation container and a capture container. Network protocols or technologies which need to be validated are deployed at the emulation container. The capture container is responsible for real-time network measurement and full packets capture in simulation scenarios. Considering changeable simulation scenarios and different networks, researchers need to set network measurement points as needed. Via binding to each other in the form of Pod and sharing network resources, the capture container can measure and monitor all network information of the emulation container. Thus, the flexible configuration of network measurement on demand is realized.

## Node Parameters
Each emulation node has corresponding physical parameters which are used to describe real network nodes in simulation scenarios. These parameters cover communication settings, location parameters, and service configurations. The web front-end in the control plane provides interactive interfaces for researchers to set these parameters. Corresponding settings are stored in a database.

The communication settings reflect the communication characteristics of the network node, e.g. effective isotropic radiated power (EIRP), G/T, modulation, communication frequency, and etc. According to the different placements of the network node in SN, the location parameters should be discussed separately. For space-based nodes, the location parameters contain eccentricity, period, inclination, right ascension of the ascending node, argument of perigee, and true anomaly. For terrestrial-based node, the location parameters refer to longitude and latitude. Through communication settings and location parameters, the calculation of connection relationships and link characteristics between nodes could be gained. Service configurations are applied to initialize the function of the emulation node, involving protocols selection, data transmission model, node types selection, and etc.

## Multiple Network Protocols
Based on different container images, various of emulation containers are created. Typically, these containers could be divided into three types, namely DTN node, TCP/IP node and custom node, as shown in Figure 4. DTN node and TCP/IP node correspond to DTN and TCP/IP network protocol architecture respectively. Furthermore, custom node provides an extension method for new or modified protocol architectures in the proposed architecture. Network protocol implementation softwares, e.g. ION-DTN, Quagga, and etc., and other applications would be deployed among container images in advance to support different requirements of emulation.

# Dynamic Link Design
The time-varying connection relationship and the dynamic link characteristic are the main features of SN. In order to analyze the dynamic behaviors, a specific SN scenario is divided into multiple time slots. The network status is regarded as static and represented by the initial state in each slot. The more time slots are divided, the more accurate of SN environment is emulated.

## Implementation of Time-varying Connection Relationship
The link types in SN can be classified into three categories, namely space-based link, ground-based link, and space-terrestrial link. Due to the mobility of space platforms, the time-varying connection relationship is mainly reflected in space-based link and space-terrestrial link. The mutual visibility between two space platforms or between a space platform and a ground-based node determines whether there exist a corresponding link. Relied on the location parameters of emulation nodes, the main controller calculates the actual position of nodes, and then achieves the mutual visibility between nodes at each time slot.

Connection relationships, which covers link_id, srcnode_port, dstnode_port, start_time and end_time, are stored in database. Link_id is the link name. Srcnode_port and dstnode_port represent the connection port of source node and destination node on virtual switches separately. The srcnode field in srcnode_port means the host name of emulation node. It is the same for dstnode_port. Start_time and end_time represent beginning and ending times of a link connection respectively.

Emulation nodes use virtual ethernet (veth) devices [30] to access the virtual switch on the server where it is located. Each veth represents an antenna or a communication channel in the network scenario. Veth devices are created in pairs. One device in the pair is assigned to the emulation node as a network interface and the other is assigned to the virtual programmable switch as a port. When either device is down, the link state of the pair is down. So, the variation of connection relationships between pair-wise nodes equates to delete/add corresponding flow entries on virtual switches to control the port up and down. But a port-based flow entry can only control the one-way link. Thus, two flow entries are needed to control a link connection, which is two-way, on or off.

The main controller later allocates corresponding network parameters for network interfaces at the emulation node, including IP addresses, MAC addresses, interface name, and etc. The neighboring veths which are connected with each other in one hop have the same subnetwork segment. Whenever corresponding forwarding rules between these veths are deployed, the links between neighboring emulation nodes are working.

Upon the emulation starts, flow entities are issued by the main controller. As shown in Figure 7, each dynamic link corresponds to a timer and two flow entities. At the start time of emulation, several timers are activated and the on/off time points of each dynamic links are set in them. When timers expire, the link connection or disconnection functions are executed to deploy corresponding connection or delete corresponding flow entities on virtual switches.

Figure 8 and Figure 9 show the change of link connection in time slot TS1 and TS2. When Node3 disconnects with Node2 and connects with Node1 directly, the main controller first deletes the bidirectional flow entries between S1-4 and S2-6 ports, and then adds the bidirectional forwarding rules between S1-2 and S2-5 ports in S1 and S2 separately. Because the forwarding rule is a map between two physical ports in the programmable switch, we can emulate the connectivity of link connections in the physical layer easily.

## Implementation of Dynamic Link Characteristic
In the emulation architecture, the SDN switch only offers the topology management and do not support the simulation of link characteristics. So, another supplementary method is needed to fulfill this function.

All emulation nodes has installed the Linux-based operation system in advance. The link characteristic configuration is done via the Linux traffic control (TC) mechanism [31]. NetEm is part of TC and supports the emulation of network delay, packet loss ratio and etc. This mechanism also includes a Token Bucket Filter (TBF) for bandwidth limitation. TC has been evaluated in [32] and has been found to emulate most parameters accurately.

TC can classify different parts of data packet according to its characteristics and provide different traffic control mechanisms for these packets. The queuing rule is one of the most important parts in TC which can be used to realize the classification function. Before packets are sent by network interfaces, they are added to different send queues according to the characteristics of packets. Then the kernel takes packets from these queues and delivers packets to network interfaces to complete the process of data transmission.

The FIFO algorithm forms the basis for the default queuing disciplines (qdisc) on all Linux network interfaces. It transmits packets as soon as it can receive and queue them, as shown in Figure 10. A real FIFO qdisc must have a size parameter to prevent it from overflowing in case it is unable to dequeue packets as quickly as it receives them, i.e. when we have a requirement to emulate a high bandwidth with a certain delay, we need to calculate the queue size to avoid discarding packets. The size of the queue is defined by the parameter limit, and the unit of limit is the packet in TC. We consider the value of limit in the worst case. The limit would be greater than the average delay t multiplied by the maximum packet rate. The maximum packet rate equals to the maximum bandwidth B divided by the maximum transmission unit (MTU). MTU is the largest length of the
frame. Its default value is 1500 bytes and this value can be changed according to the need
of experiments.

# Network Topology Design
The network topology of the emulation architecture can be divided into the lower physical network topology and the upper logical network topology.

## Lower Physical Network Topology
The lower physical network topology is the basis of emulation system, which is constructed by the main controller, switches, and servers. For each server in data plane, a virtual switch is implemented. Some network interfaces of the server are attached to the virtual switch. To communicate with other servers, other ends of attached network interfaces are connected to a high performance SDN hardware switch in the form of Virtual Extensible LAN. In this way, a distributed server cluster is constructed in a star structure. The main controller is linked to virtual switches and SDN hardware switches, and it controls them directly. When researchers create a specific network scenario in the logical plane, the main controller, integrated with container orchestration, allocates resources and places emulation nodes at the server cluster. Emulation nodes are linked to a virtual switch at each servers in a star network topology. Physical nodes and external emulation devices can also be linked to the SDN hardware switch and managed by the main controller. From top to bottom, all the switches, servers, and emulation nodes construct in a hierarchical star network structure. This network topology makes the platform have high scalability. If computing resources need to be expanded, more virtual node servers can be simply accessed through the SDN hardware switch.

## Upper Logical Network Topology
The upper logical network topology keep consistent with the SN emulation scenario. With the scene information, including time-varying connection relationships, dynamic link characteristics, emulation node parameters, and etc, the main controller generates corresponding commands, such as flow entities. Then data plane is driven by these commands. In this way, the proposed platform emulates a network scenario with arbitrary logical topology described in logical plane.
 
# Conclusions
Through the above designs, the proposed emulation platform effectively provides a realistic, flexible, and extensible experimental environment for SN. 

Virtual emulation nodes have the same functionality as physical hardware and can execute exactly the same code in a real deployment. Meanwhile, the proposed emulation architecture supports the joint emulation method among virtual nodes, physical nodes, real networks and real links, bringing credible emulation results.

Integrating with SDN and traffic control mechanism, the networking can change automatically like that of a real space communication environment (especially, time-varying link quality and dynamic link connection). And various network protocol architectures are reconstructed by container images. These realize the reliable and flexible emulation of different SN scenarios.

The platform architecture has extendibility including the horizontal extension of hardware resources and the access of external physical emulation devices. The main controller offers a novel unified control for the connection relationships between emulation nodes under different switches through flow tables. These features allow multiple nodes with huge individual differences to become a part of the proposed platform and to be managed.
 
# Security Considerations
This document raises no security considerations.

# IANA Considerations
This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
