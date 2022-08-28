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
 
# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
