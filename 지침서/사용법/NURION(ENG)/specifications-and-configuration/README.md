---
description: Nurion System Specifications and Configuration
---

# Nurion System Specifications and Configuration

## **A. Overview**

Nurion, which is KISTI's 5th supercomputer, is a Linux-based massively parallel cluster system that exhibits a total theoretical operation performance (Rpeak) of 25.7 PFlops (approximately 70 times that of the 4th supercomputer, Tachyon2).

&#x20;

Nurion consists of 8,305 manycore CPUs (consisting of 68 cores) that have a performance of 3 TFlops (1/100 of the overall performance of Tachyon2, the 4th supercomputer) per socket (CPU) and is consequently a cluster-based supercomputing system in which tens of thousands of cores can be utilized simultaneously. It is also equipped with high-performance interconnect, mass storage, and a burst buffer. A burst buffer can efficiently process the massive I/O requests that occur instantly in application programs.

&#x20;

Nurion aims to help improve national R\&D competence based on its large-scale computational capability and to solve complicated problems such as weather prediction or new material development as well as expensive and dangerous experiments involving aviation or new drug development. In an effort to solve different problems related to scientific technology, difficult problems are being discovered across a variety of fields even in the planning phase.

&#x20;

Various types of software are equipped and consistently upgraded in Nurion to support R\&D efforts, while high-performance hardware are linked to maintain the highest productivity. Furthermore, Nurion intends to become essential infrastructure for implementing an intelligent information society and advancing national scientific technology through user support such as through optimization, parallelization, and large project discovery for solving significant challenges.

&#x20;

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=hZUlzJMzieep1fc.png)

&#x20;

\[Block diagram of Nurion]

&#x20;

| Category                                   | KNL Computing node                                                     | CPU-only node            |                           |
| ------------------------------------------ | ---------------------------------------------------------------------- | ------------------------ | ------------------------- |
| Manufacturer and model                     | Cray CS500                                                             |                          |                           |
| No. of nodes                               | 8,305                                                                  | 132                      |                           |
| Theoretical peak performance (Rpeak)       | 25.3 PFlops                                                            | 0.4 PFlops               |                           |
| Processor                                  | Model                                                                  | Intel Xeon Phi 7250(KNL) | Intel Xeon 6148 (Skylake) |
| Theoretical performance per CPU            | 3.0464 TFLOPS                                                          | 1.536 TFLOPS             |                           |
| No. of cores per CPU                       | 68                                                                     | 20                       |                           |
| No. of CPUs per node                       | 1                                                                      | 2                        |                           |
| On-package Memory                          | 16GB, 490GB/s                                                          | -                        |                           |
| Main memory                                | Model                                                                  | 16GB DDR4-2400           | 16GB DDR4-2666            |
| Configuration                              | 16GB x6, 6Ch per CPU                                                   | 16GB x12, 6Ch per CPU    |                           |
| Memory per node (GB)                       | 96GB                                                                   | 192GB                    |                           |
| Bandwidth per CPU                          | 115.2GB/s                                                              | 128GB/s                  |                           |
| Total size                                 | 778.6TB                                                                | 24.8TB                   |                           |
| High-performance interconnect              | Topology                                                               | Fat Tree                 |                           |
| Blocking Ratio                             | 50% Blocking                                                           |                          |                           |
| Switches configuration                     | <p>278x 48-port OPA edge switches<br>8x 768-port OPA core switches</p> |                          |                           |
| Bandwidth per port                         | 100Gbps                                                                |                          |                           |
| High-performance filesystem (burst buffer) | No. of servers                                                         | DDN IME240 Server 48ea   |                           |
| Disk per server                            | 16x 1.2TB NVMe SSD, 2x 0.45TB NVMe SSD                                 |                          |                           |
| Total available capacity                   | 0.8PB                                                                  |                          |                           |
| Bandwidth                                  | 20GB/sec per server, 0.8TB/s total                                     |                          |                           |
| Parallel filesystem                        | File system                                                            | Lustre 2.7.21.3          |                           |
| Total available capacity                   | /scratch: 21PB, /home: 0.76PB, /apps: 0.5PB                            |                          |                           |
| Bandwidth                                  | 0.3TB/s                                                                |                          |                           |
| RAID configuration                         | RAID6(8D+2P)                                                           |                          |                           |

&#x20;

\[Nurion system specifications and configuration]

&#x20;

## B. Computing Node

There are a total of 8,437 computing nodes in the Nurion system, among which 8,305 are equipped with the manycore-based Intel Xeon Phi 7250 processor, whereas 132 of them are equipped with the Intel Xeon Gold 6148 processor.

&#x20;

#### KNL (manycore) node

The Intel Xeon Phi 7250 processor (code name: Knights Landing) equipped in the KNL node is operated by 68 cores (hyper threading off) at a fundamental frequency of 1.4 GHz. The L2 cache memory is 34 MB, and the Multi-Channel DRAM (MCDRAM), which is an on-package memory, is 16 GB (490 GB/s bandwidth). The memory per node is 96 GB, consisting of six channels of 16 GB DDR4-2400 memory. The 2U-size enclosure is equipped with four computing nodes, in which the 42U standard rack consists of 72 computing nodes.

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=M9lQKV9UwdVVho0.png)

\[Block diagram of KNL-based computing node]

&#x20;

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=8eTQgXRi3rFKRQO.png)

\[KNL-based computing node]

&#x20;

#### SKL (CPU-only) node

The SKL (CPU-only) node is equipped with two Intel Xeon Gold 6148 processors (code name: Skylake). The fundamental frequency is 2.4 GHz, and there are 20 CPU cores (hyperthreading off). The L3 cache memory is 27.5 MB, and the memory per CPU is 96 GB (192 GB per node) with six channels of 16 GB DDR4-2666 memory. The 2U-size enclosure is equipped with four computing nodes.

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=NwiAvTQB3n1mbjQ.png)

\[Block diagram of SKL-based CPU-only node]

&#x20;

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=h3jV5a33UZiVL0I.png)

\[SKL-based CPU-only node]

&#x20;

## C. Interconnect Network

It uses Intel’s Omni-Path Architecture (OPA) as the computing network between nodes and the interconnect network for file I/O communications. In a fat-tree topology using 100 Gbps OPA, the network is configured with 50% blocking between the computing nodes and non-blocking between storage units. The computing nodes and storage units are connected to 277 48-port OPA Edge switches, and all Edge switches are connected to eight 768-port OPA Director switches.

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=89LSApl4oE0nAN0.png)

\[Block diagram of interconnect network]

&#x20;

## D. Storage

#### Parallel filesystem and burst buffer configuration

Nurion provides a parallel filesystem and burst buffer configuration for I/O processing and data storage. Three Lustre filesystems, namely scratch (/scratch 21PB), home (/home01, 760TB), and application (/app, 507TB), are provided for the parallel filesystem.

&#x20;

Each filesystem consists of an SFA7700X storage-based metadata server (MDS) and an ES14KX storage-based object storage server (OSS). The burst buffer is an NVMe based I/O cache that acts between computing nodes and the parallel filesystem and provides approximately 844 TB capacity. The Lustre filesystem provides I/O services by being mounted on the computing node, login node, and archiving server (data mover).

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=sOdKlUWMwnbB9lP.png)

\[Configuration of parallel filesystem and overall burst buffer rack]

&#x20;

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2021/11/\&name=UikUtB7HWx9PSti.png)

\[PFS server configuration]

&#x20;

#### Burst Buffer

\- Overview

Burst buffer (hereinafter, “BB”) is a cache layer between the computing node and storage (/scratch) for I/O acceleration; it was newly introduced in the 5th supercomputer to maximize the performance of the parallel I/O and supplement the low performance for small or random I/O in the parallel filesystem. It aims to enhance the performance of user applications with high I/O dependency and supports all queues using KNL nodes.

&#x20;

![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2020/02/\&name=6n7FDTWgDLwFPaD.png) ![](https://www.ksc.re.kr/file/image/?path=sos/jcs/2020/02/\&name=ijCBgRkhSskY4Fv.png)

&#x20;                  \[Burst Buffer role]                       \[Burst Buffer server configuration]

&#x20;

\- System configuration

The IME240 solution of DDN was applied for the BB configuration; the \[Burst buffer server configuration] figure above shows the detailed configuration of the BB.

&#x20;

| System               | DDN IME240, Infinite Memory Engine Appliance |
| -------------------- | -------------------------------------------- |
| Software version     | CentOS 7.4, IME 1.3                          |
| Max. I/O performance | 20 GB/s                                      |
| Network interface    | 2 x OPA                                      |
| SSD type             | 1.2TB, NVMe drive                            |
| SDD quantity         | 16ea(1.2TB NVMe) + 1ea(450GB SSD)            |
| Capacity (RAW)       | 19.2TB                                       |

&#x20;

\[IME single node configuration]

&#x20;

&#x20;

| Total no. of systems                     | IME240 x 48         |
| ---------------------------------------- | ------------------- |
| Total capacity (RAW)                     | 979.2 TB            |
| Total capacity (when parity is applied)) | 816 TB (EC\*, 10+2) |
| Max. I/O performance                     | 800 GB/s            |

&#x20;

\[Overall configuration of burst buffer]

&#x20;

\*EC : Erasure Coding (settings may change)

&#x20;

&#x20;

2021년 11월 29일에 마지막으로 업데이트되었습니다.
