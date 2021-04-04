---
title: "Communication: Applications and Privacy"
author: Yafu Ruan <yr942@nyu.edu>
---
# Introduction
Present an overview of the problem and the approaches.
1. BASTION is designed to secure contain network through an intelligent contain-aware communication sandbox.
2. 
3.

# BASTION
## Motivation
Currently there are different container networks including Docker Platform, Kubernetes Orchestration System and Network-privilegesd Containers. There are some underlying architectural limitations of network security in those networks:
1. Loss of container context: We do not know where the packet actually from in the host network namespace
2. Limitation of IP-based access controls: IP address of containers can be dynamic, and adjuestment are required whenever containers are spun up and down.
3. Network policy explosion: iptables is a centralized mechanism for all network interfaces in this host, and it results in monolithic network rules, which makes the number of policies in iptables increases rapidly. This may cause performance degradation in the container ecosystem.
4. Unrestricted host access: Since the gateway of the container network is located in the host network namespace, a container can directly access thh sergive through gateway IP address.
5. No restriction on network-privileged containers: Network-privileged containers can access not only the host network interfaces, but can also monitor all network traffic from deployed containers in the host and are unrestrained in their ability to inject malicious packets into container networks.

The goal of Bastion is to:
1. Protect network threats that abuse the security challenges of current container networks
2. Isolate inter-container communications according to their dependencies

## Approaches

There are three key components in Bastion:
1. Bastion manager: Collect all network information from container platforms.
2. Network visibility service: Provide fine-grained control over different network topology visibility per container application.
3. Traffic visibility service: Securely isolate inter-container communication in a point-to-point manner and prevent the exposure of inter-container network traffic to other peer containers.

![Bastion overview](images/bastion_overview.png)

### Manager
1. Container Collection:
The manager maintains two hash maps for network and inter-container dependency. It collects the network information of deployed containers from contrainer platforms, and extract the inter-container dependencies using container from configurations and network policies.
2. Security Stack Management:
It installs the network stacks at their interfaces for new containers. The maps of security stack is updated in run time.

![security_stack](images/security_stack.png)

### Network Visibility Service

The network visibility service restricts unnecessary connectivity among containers and between containers and external hosts. Three components are introduced here to accomplish it:
1. Direct ARP handler:
BASTION’s direct ARP handler filters out any unnecessary container discovery that does not pertain to the present container’s dependency map. When a container sends an ARP request, the handler intercepts the request before it is broadcasted, verifying if the source container has a dependency on the destination container.

![arp handler](images/arp.png)

2. Inter-container Communications Handler:
It implements containeer-aware network isolation to address malicious access among dependent containers.
3. Gateway and ervice-IP Handler:
It filters direct host accesses to prevent a subverted container to exploit the gateway to probe services within the host OS.

### Traffic Visibility Service
The traffic visibility service provides point-to-point integrity and confidentiality among container network flows. BASTION hides irrelevant traffic from containers using two security components: source verification and end-to-end direct forwarding.

1. Source Verification: BASTION verifies the incoming traffic by comparing not only the packet header information but also its metadata to the container’s information embedded in the BASTION network stack. If either the packet header information or the metadata is not matched with the container network information, BASTION identifies the incoming traffic as spoofed and drop it.

2. End-to-end Direct Forwarding: It directly inject packets delivered from a source and bypass the container network


## Trade-Offs
## Open Questions and Future Work

# FaRM
## Motivation
## Approaches
## Trade-Offs
## Open Questions and Future Work



# Mostly-ordered multicast
## Motivation
## Approaches
## Trade-Offs
## Open Questions and Future Work









# References:
1. https://cs.nyu.edu/~apanda/classes/sp21/papers/bastion.pdf
2. https://www.usenix.org/system/files/atc20-paper166-slides-nam.pdf
3. 

# Motivation
What is the problem that the papers are looking at, and why are they looking at
this? 
This is a bit harder for the two introduction lectures, since they try to
overview the area.

# Approaches
Talk a bit about the approaches the papers discussed, and what you found from
your research.

# Trade-Offs
Compare the approaches, discuss when they might be appropriate. For example,
some techniques might only make sense with a lot of data, or in the absence of
multiple tenants, etc.

# Open Questions and Future Work
How are the current trends affecting the area, what are some open questions
about the problem, etc.
