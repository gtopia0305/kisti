---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Nurion System Specifications and
  Configuration > B. Computing Node
---

# B. Computing Node

There are a total of 8,437 computing nodes in the Nurion system, among which 8,305 are equipped with the manycore-based Intel Xeon Phi 7250 processor, whereas 132 of them are equipped with the Intel Xeon Gold 6148 processor.

&#x20;

#### KNL (manycore) node

The Intel Xeon Phi 7250 processor (code name: Knights Landing) equipped in the KNL node is operated by 68 cores (hyper threading off) at a fundamental frequency of 1.4 GHz. The L2 cache memory is 34 MB, and the Multi-Channel DRAM (MCDRAM), which is an on-package memory, is 16 GB (490 GB/s bandwidth). The memory per node is 96 GB, consisting of six channels of 16 GB DDR4-2400 memory. The 2U-size enclosure is equipped with four computing nodes, in which the 42U standard rack consists of 72 computing nodes.

![\[Block diagram of KNL-based computing node\]](<../../../../.gitbook/assets/KNL 기반 계산노드 블록 다이어그램.png>)

![\[KNL-based computing node\]](<../../../../.gitbook/assets/KNL 기반 계산노드.png>)

#### SKL (CPU-only) node

The SKL (CPU-only) node is equipped with two Intel Xeon Gold 6148 processors (code name: Skylake). The fundamental frequency is 2.4 GHz, and there are 20 CPU cores (hyperthreading off). The L3 cache memory is 27.5 MB, and the memory per CPU is 96 GB (192 GB per node) with six channels of 16 GB DDR4-2666 memory. The 2U-size enclosure is equipped with four computing nodes.

![\[Block diagram of SKL-based CPU-only node\]](<../../../../.gitbook/assets/SKL 기반 CPU-only 노드 블록 다이어그램.png>)

![\[SKL-based CPU-only node\]](<../../../../.gitbook/assets/SKL 기반 CPU-only 노드.png>)
