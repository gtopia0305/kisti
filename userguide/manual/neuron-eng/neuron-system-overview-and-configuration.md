# Neuron System Overview and Configuration

## A. Purpose of System Operation

\* It was decided that No.5 system would be a Knights Landing-based system. Consequently, a GPU-based system is operated to accommodate various user demands.

\* Large capacity memory nodes are operated to meet the demand of jobs requiring large memory.

## B. System Configuration

**○ Compute nodes**

**- GPU nodes**

| **Category**     | **Specification**                                               |                                                       |
| ---------------- | --------------------------------------------------------------- | ----------------------------------------------------- |
| Model            | Lenovo nx360-m4                                                 |                                                       |
| Operating system | CentOS 7.9 (Linux, 64-bit)                                      |                                                       |
| Number of nodes  | TESLA V100                                                      | 21 nodes (2 V100 cards per node, HBM2 16 GB per card) |
| CPU              | Intel Xeon Ivy Bridge (E5-2670) / 2.50 GHz (10-core) / 2 socket |                                                       |
| Main memory      | 128 GB DDR3 Memory per node                                     |                                                       |

| **Category**        | **Specification**                                                   |                                                      |
| ------------------- | ------------------------------------------------------------------- | ---------------------------------------------------- |
| Model               | HP Apollo 6500 Gen10                                                |                                                      |
| Operating system    | CentOS 7.9 (Linux, 64-bit)                                          |                                                      |
| Number of nodes     | TESLA V100                                                          | 15 nodes (2 V100 cards per node, HBM2 32GB per card) |
| TESLA V100 (NVlink) | 4 nodes (4 V100 cards per node, HBM2 32 GB per card)                |                                                      |
| CPU                 | Intel Xeon Cascade Lake (Gold 6230) / 2.1 0GHz (20-core) / 2 socket |                                                      |
| Main memory         | 384 GB DDR4 Memory per node                                         |                                                      |

| **Category**        | **Specification**                                                    |                                                      |
| ------------------- | -------------------------------------------------------------------- | ---------------------------------------------------- |
| Model               | Fujitsu GX2570M5, RX2540M5                                           |                                                      |
| Operating system    | CentOS 7.9 (Linux, 64-bit)                                           |                                                      |
| Number of nodes     | TESLA V100                                                           | 4 nodes (2 V100 cards per node, HBM2 32 GB per card) |
| TESLA V100 (NVlink) | 5 nodes (8 V100 cards per node, HBM2 32 GB per card)                 |                                                      |
| CPU                 | Intel Xeon Cascade Lake (Gold 6226R) / 2.90 GHz (16-core) / 2 socket |                                                      |
| Main memory         | 384 GB DDR4 Memory per node                                          |                                                      |

**- CPU\_only nodes**

| **Category**     | **Specification**                                              |
| ---------------- | -------------------------------------------------------------- |
| Model            | Dell R640                                                      |
| Operating system | CentOS 7.9 (Linux, 64-bit)                                     |
| Number of nodes  | 10 nodes                                                       |
| CPU              | Intel Xeon Skylake (Gold 6140) / 2.30 GHz (18-core) / 2 socket |
| Main memory      | 192 GB DDR4 Memory per node                                    |

**- High-capacity memory nodes**

| **Category**     | **bigmem01**                                                 | **bigmem02**                                                  |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------- |
| Model            | Oracle X4480M2                                               | Fujitsu RX4770 M3                                             |
| Operating system | CentOS 7.9 (Linux, 64-bit)                                   |                                                               |
| Number of nodes  | 1 node                                                       | 1 node                                                        |
| CPU              | Intel Xeon Westmere (E7-4870) / 2.40 GHz(10-core) / 4 socket | Intel Xeon Broadwell (E7-4830) / 2.00 GHz(14-core) / 4 socket |
| Memory           | 512 GB DDR3 Memory per node                                  | 768 GB DDR4 Memory per node                                   |

**- Next-generation architecture test nodes**

| **Category**     | **amd**                            | **optane**                                                    |
| ---------------- | ---------------------------------- | ------------------------------------------------------------- |
| Model            | Supermicro AS-1023US-TR4           | Supermicro SYS-2029U-TR4                                      |
| Operating system | CentOS 7.9 (Linux, 64-bit)         |                                                               |
| Number of nodes  | 2 nodes                            | 1 node                                                        |
| CPU              | AMD EPYC 7542 (32-core) / 2 socket | Intel Cascade Lake (Gold 6246) / 3.30 GHz(12-core) / 2 socket |
| Memory           | 256 GB DDR4 Memory per node        | 1.5 TB Optane Persistent Memory per node                      |

**○ Storage configuration**

Neuron storage is a DDN SFA12K-40 model and consists of 10 Enclosure SS8460 (Disk Box) and 10 Oracle X3-2L servers (2 MDS, 8 OSS). The Neuron storage is configured using a Lustre parallel distributed file system, and the actual available capacity is about 2.3 PB. The home directory (/home01), application directory (/apps), and scratch directory (/scratch2) are configured separately in a single file system and are mounted on login and compute nodes.

![\[ Neuron storage configuration diagram\]](<../../../.gitbook/assets/TkbLovYt7ZryPFB (1).png>)

| <p>Parallel file system<br>(Lustre)</p> | File system   | Lustre 2.10.5 |
| --------------------------------------- | ------------- | ------------- |
| Total available capacity                | 2.3 PB        |               |
| Bandwidth                               | 25 GB/s       |               |
| RAID configuration                      | RAID6 (8D+2P) |               |
