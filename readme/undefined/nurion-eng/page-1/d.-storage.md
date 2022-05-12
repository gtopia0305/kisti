---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Nurion System Specifications and
  Configuration > D. Storage
---

# D. Storage

#### Parallel filesystem and burst buffer configuration

Nurion provides a parallel filesystem and burst buffer configuration for I/O processing and data storage. Three Lustre filesystems, namely scratch (/scratch 21PB), home (/home01, 760TB), and application (/app, 507TB), are provided for the parallel filesystem.

&#x20;

Each filesystem consists of an SFA7700X storage-based metadata server (MDS) and an ES14KX storage-based object storage server (OSS). The burst buffer is an NVMe based I/O cache that acts between computing nodes and the parallel filesystem and provides approximately 844 TB capacity. The Lustre filesystem provides I/O services by being mounted on the computing node, login node, and archiving server (data mover).

![\[Configuration of parallel filesystem and overall burst buffer rack\]](<../../../../.gitbook/assets/병렬파일시스템 및 버스트버퍼 전체 랙 구성.png>)

![\[PFS server configuration\]](<../../../../.gitbook/assets/PFS 서버 구성.png>)

#### Burst Buffer

\- Overview

Burst buffer (hereinafter, “BB”) is a cache layer between the computing node and storage (/scratch) for I/O acceleration; it was newly introduced in the 5th supercomputer to maximize the performance of the parallel I/O and supplement the low performance for small or random I/O in the parallel filesystem. It aims to enhance the performance of user applications with high I/O dependency and supports all queues using KNL nodes.

![\[Burst Buffer role\]](<../../../../.gitbook/assets/Burst Buffer 역할.png>) ![  \[Burst Buffer server configuration\]](<../../../../.gitbook/assets/Burst Buffer 서버 구성.png>)



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
