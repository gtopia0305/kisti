---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Nurion System Specifications and
  Configuration > C. Interconnect Network
---

# C. Interconnect Network

It uses Intel’s Omni-Path Architecture (OPA) as the computing network between nodes and the interconnect network for file I/O communications. In a fat-tree topology using 100 Gbps OPA, the network is configured with 50% blocking between the computing nodes and non-blocking between storage units. The computing nodes and storage units are connected to 277 48-port OPA Edge switches, and all Edge switches are connected to eight 768-port OPA Director switches.

![\[Block diagram of interconnect network\]](<../../../../.gitbook/assets/인터커넥트 네트워크 구성도.png>)
