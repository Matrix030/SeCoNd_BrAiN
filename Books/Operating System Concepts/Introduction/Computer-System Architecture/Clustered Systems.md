---
tags: [book, os, operating-systems, book-os-concepts, networking]
aliases: ["1.3.3"]
---

# **1.3.3 Clustered Systems**

1) Another type of multiple-CPU system is the **clustered system**. Like multiprocessor systems, clustered systems gather together multiple CPUs to accomplish computational work. 
2) Clustered systems differ from multiprocessor systems, however, in that they are composed of two or more individual systems—or nodes—joined together. The generally accepted definition is that clustered computers share storage and are closely linked via a **local-area network ([[LAN]])** (as described in Section 1.10) or a faster interconnect, such as InfiniBand.
3) Clustering is usually used to provide **high-availability** service; that is, service will continue even if one or more systems in the cluster fail. High availability is generally obtained by adding a level of redundancy in the system. A layer of cluster software runs on the cluster nodes. Each node can monitor one or more of the others (over the [[LAN]]). If the monitored machine fails, the monitoring machine can take ownership of its storage and restart the applications that were running on the failed machine. The users and clients of the applications see only a brief interruption of service.

> [!note] Beowulf Clusters
> 1) Beowulf clusters are designed for solving high-performance computing tasks. These clusters are built using commodity hardware—such as personal computers—that are connected via a simple local area network. 
> 2) Interestingly, a Beowulf cluster uses no one specific software package but rather consists of a set of open-source software libraries that allow the computing nodes in the cluster to communicate with one another. 
> 3) Thus, there are a variety of approaches for constructing a Beowulf cluster, although Beowulf computing nodes typically run the Linux operating system. Since Beowulf clusters require no special hardware and operate using open-source software that is freely available, they offer a low-cost strategy for building a high-performance computing cluster. 
> 4) In fact, some Beowulf clusters built from collections of discarded personal computers are using hundreds of computing nodes to solve computationally expensive problems in scientific computing.

5) Clustering can be structured asymmetrically or symmetrically. 
	1) In **asymmetric clustering**, one machine is in **hot-standby mode** while the other is running the applications. The hot-standby host machine does nothing but monitor the active server. If that server fails, the hot-standby host becomes the active server. 
	2) In **symmetric mode**, two or more hosts are running applications and are monitoring each other. This mode is obviously more efficient, as it uses all of the available hardware. It does require that more than one application be available to run.
6) As a cluster consists of several computer systems connected via a network, clusters may also be used to provide **high-performance computing** environments. Such systems can supply significantly greater computational power than single-processor or even [[SMP]] systems because they are capable of running an application concurrently on all computers in the cluster.
7) However, applications must be written specifically to take advantage of the cluster by using a technique known as **parallelization**, which consists of dividing a program into separate components that run in parallel on individual computers in the cluster. 
8) Typically, these applications are designed so that once each computing node in the cluster has solved its portion of the problem, the results from all the nodes are combined into a final solution.
9) Other forms of clusters include parallel clusters and clustering over a wide-area network (WAN) (as described in Section 1.10). Parallel clusters allow multiple hosts to access the same data on the shared storage. 
10) Because most operating systems lack support for simultaneous data access by multiple hosts, parallel clusters are usually accomplished by use of special versions of software and special releases of applications. 
11) For example, Oracle Real Application Cluster is a version of Oracle's database that has been designed to run on a parallel cluster. Each machine runs Oracle, and a layer of software tracks access to the shared disk. Each machine has full access to all data in the database. To provide this shared access to data, the system must also supply access control and locking to ensure that no conflicting operations occur. This function, commonly known as a **distributed lock manager ([[DLM]])**, is included in some cluster technology.

Cluster technology is changing rapidly. Some cluster products support dozens of systems in a cluster, as well as clustered nodes that are separated by miles. Many of these improvements are made possible by **storage-area networks ([[SAN]]s)**, as described in Section 12.3.3, which allow many systems to attach to a pool of storage. If the applications and their data are stored on the [[SAN]], then the cluster software can assign the application to run on any host that is attached to the [[SAN]]. If the host fails, then any other host can take over. In a database cluster, dozens of hosts can share the same database, greatly increasing performance and reliability.

![[Pasted image 20260526154618.png]]

## Related

- [[> Computer-System Architecture]] — back to the sub-topic MOC
- [[Multiprocessor Systems]] — closely related multiple-CPU approach
- [[Single-Processor Systems]] — the contrast case
