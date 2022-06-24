# Nurion System Specifications and Configuration

## **A. Overview**

Nurion, which is KISTI's 5th supercomputer, is a Linux-based massively parallel cluster system that exhibits a total theoretical operation performance (Rpeak) of 25.7 PFlops (approximately 70 times that of the 4th supercomputer, Tachyon2).

Nurion consists of 8,305 manycore CPUs (consisting of 68 cores) that have a performance of 3 TFlops (1/100 of the overall performance of Tachyon2, the 4th supercomputer) per socket (CPU) and is consequently a cluster-based supercomputing system in which tens of thousands of cores can be utilized simultaneously. It is also equipped with high-performance interconnect, mass storage, and a burst buffer. A burst buffer can efficiently process the massive I/O requests that occur instantly in application programs.

Nurion aims to help improve national R\&D competence based on its large-scale computational capability and to solve complicated problems such as weather prediction or new material development as well as expensive and dangerous experiments involving aviation or new drug development. In an effort to solve different problems related to scientific technology, difficult problems are being discovered across a variety of fields even in the planning phase.

Various types of software are equipped and consistently upgraded in Nurion to support R\&D efforts, while high-performance hardware are linked to maintain the highest productivity. Furthermore, Nurion intends to become essential infrastructure for implementing an intelligent information society and advancing national scientific technology through user support such as through optimization, parallelization, and large project discovery for solving significant challenges.

![\[Block diagram of Nurion\]](../../../.gitbook/assets/46cGZjtaR1zSlum.png)

![\[Nurion system specifications and configuration\]](../../../.gitbook/assets/nurion-system-specifications-and-configuration.png)

## B. Computing Node

There are a total of 8,437 computing nodes in the Nurion system, among which 8,305 are equipped with the manycore-based Intel Xeon Phi 7250 processor, whereas 132 of them are equipped with the Intel Xeon Gold 6148 processor.

#### KNL (manycore) node

The Intel Xeon Phi 7250 processor (code name: Knights Landing) equipped in the KNL node is operated by 68 cores (hyper threading off) at a fundamental frequency of 1.4 GHz. The L2 cache memory is 34 MB, and the Multi-Channel DRAM (MCDRAM), which is an on-package memory, is 16 GB (490 GB/s bandwidth). The memory per node is 96 GB, consisting of six channels of 16 GB DDR4-2400 memory. The 2U-size enclosure is equipped with four computing nodes, in which the 42U standard rack consists of 72 computing nodes.

![\[Block diagram of KNL-based computing node\]](../../../.gitbook/assets/pOeRBFHIcyQUwfC.png)

![\[KNL-based computing node\]](../../../.gitbook/assets/MteLQQNt86MTMEz.png)

#### SKL (CPU-only) node

The SKL (CPU-only) node is equipped with two Intel Xeon Gold 6148 processors (code name: Skylake). The fundamental frequency is 2.4 GHz, and there are 20 CPU cores (hyperthreading off). The L3 cache memory is 27.5 MB, and the memory per CPU is 96 GB (192 GB per node) with six channels of 16 GB DDR4-2666 memory. The 2U-size enclosure is equipped with four computing nodes.

![\[Block diagram of SKL-based CPU-only node\]](../../../.gitbook/assets/NwiAvTQB3n1mbjQ.png)

![\[SKL-based CPU-only node\]](../../../.gitbook/assets/h3jV5a33UZiVL0I.png)

## C. Interconnect Network

It uses Intel’s Omni-Path Architecture (OPA) as the computing network between nodes and the interconnect network for file I/O communications. In a fat-tree topology using 100 Gbps OPA, the network is configured with 50% blocking between the computing nodes and non-blocking between storage units. The computing nodes and storage units are connected to 277 48-port OPA Edge switches, and all Edge switches are connected to eight 768-port OPA Director switches.

![\[Block diagram of interconnect network\]](../../../.gitbook/assets/89LSApl4oE0nAN0.png)

## D. Storage

#### Parallel filesystem and burst buffer configuration

Nurion provides a parallel filesystem and burst buffer configuration for I/O processing and data storage. Three Lustre filesystems, namely scratch (/scratch 21PB), home (/home01, 760TB), and application (/app, 507TB), are provided for the parallel filesystem.

Each filesystem consists of an SFA7700X storage-based metadata server (MDS) and an ES14KX storage-based object storage server (OSS). The burst buffer is an NVMe based I/O cache that acts between computing nodes and the parallel filesystem and provides approximately 844 TB capacity. The Lustre filesystem provides I/O services by being mounted on the computing node, login node, and archiving server (data mover).

![\[Configuration of parallel filesystem and overall burst buffer rack\]](../../../.gitbook/assets/sOdKlUWMwnbB9lP.png)

![\[PFS server configuration\]](../../../.gitbook/assets/UikUtB7HWx9PSti.png)

#### Burst Buffer

\- Overview

Burst buffer (hereinafter, “BB”) is a cache layer between the computing node and storage (/scratch) for I/O acceleration; it was newly introduced in the 5th supercomputer to maximize the performance of the parallel I/O and supplement the low performance for small or random I/O in the parallel filesystem. It aims to enhance the performance of user applications with high I/O dependency and supports all queues using KNL nodes.

![\[Burst Buffer role\]](../../../.gitbook/assets/6n7FDTWgDLwFPaD.png) ![\[Burst Buffer server configuration\]](../../../.gitbook/assets/ijCBgRkhSskY4Fv.png)

\- System configuration

The IME240 solution of DDN was applied for the BB configuration; the \[Burst buffer server configuration] figure above shows the detailed configuration of the BB.

| **System**               | DDN IME240, Infinite Memory Engine Appliance |
| ------------------------ | -------------------------------------------- |
| **Software version**     | CentOS 7.4, IME 1.3                          |
| **Max. I/O performance** | 20 GB/s                                      |
| **Network interface**    | 2 x OPA                                      |
| **SSD type**             | 1.2TB, NVMe drive                            |
| **SDD quantity**         | 16ea(1.2TB NVMe) + 1ea(450GB SSD)            |
| **Capacity (RAW)**       | 19.2TB                                       |

&#x20;                                                          \[IME single node configuration]

| **Total no. of systems**                     | IME240 x 48         |
| -------------------------------------------- | ------------------- |
| **Total capacity (RAW)**                     | 979.2 TB            |
| **Total capacity (when parity is applied))** | 816 TB (EC\*, 10+2) |
| **Max. I/O performance**                     | 800 GB/s            |

&#x20;                                                          \[Overall configuration of burst buffer]



\*EC : Erasure Coding (settings may change)

2021년 11월 29일에 마지막으로 업데이트되었습니다.
