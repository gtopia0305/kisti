---
description: 기술지원 > 지침서 > 사용법 > 누리온 > 누리온 시스템 사양 및 구성 > 가. 개요
---

# 가. 개요

KISTI 슈퍼컴퓨터 5호기 누리온(NURION)은 리눅스 기반의 초병렬 클러스터 시스템으로 이론최고성능(Rpeak) 25.7PFlops(4호기 Tachyon2의 약 70배)의 연산처리 성능을 갖고 있다.

&#x20;

누리온은 1소켓(CPU)당 성능이 3TFlops(4호기 Tachyon2 전체 성능의 약 1/100)인 매니코어 CPU(68개의 코어로 구성) 8,305개로 구성되어 수십만 코어를 동시에 활용할 수 있는 클러스터 방식의 초고성능 컴퓨팅 시스템이다. 그리고 고성능 인터커넥트(Interconnect), 대용량 스토리지, 버스트 버퍼(Burst Buffer)가 탑재되어 있다. 버스트 버퍼는 응용 프로그램에서 순간적으로 발생하는 대규모 I/O 요청을 원활히 처리 할 수 있다.

&#x20;

누리온은 이러한 대규모 계산 능력을 통해 국가 연구개발(R\&D)의 경쟁력 향상 기여에 목적을 두며, 기후 예측이나 신소재 개발 등 복잡한 문제나 신약개발, 항공 실험 등 실험적으로만 해결하기에는 위험하고 비용이 많이 소요되는 문제를 해결하는 데 기여하고자 한다. 누리온은 이러한 과학기술 문제를 해결하기 위해 기획 단계부터 다양한 분야의 난제를 발굴하는 노력을 병행하고 있다.

&#x20;

또한 누리온은 이러한 연구 개발들을 지원하기 위한 다양한 종류의 소프트웨어를 탑재하고 지속적으로 업그레이드 하고 있으며, 고성능 하드웨어와 유기적인 연동을 통해 최고의 생산성을 유지하기 위해 노력하고 있다. 아울러 최적ㆍ병렬화를 비롯한 다양한 사용자 지원과 거대 문제 및 난제 해결 등을 위한 대형 과제 발굴을 통해서 국가 과학기술 발전과 지능정보사회 구현의 핵심 인프라로서 역할을 수행하고자 한다.

![\[누리온 구성도\]](<../../../../.gitbook/assets/누리온 구성도.png>)

<table class="common-table-03 txt-ac" border="0" cellspacing="0" cellpadding="0"><colgroup><col style="width: 30%;"></colgroup><colgroup><col style="width: 30%;"></colgroup><colgroup><col style="width: 20%;"></colgroup><colgroup><col style="width: 20%;"></colgroup>
<thead>
<tr>
<th style="width: 415px;" colspan="2">구분</th>
<th style="width: 124px;">KNL 계산노드</th>
<th style="width: 124px;">Cpu-only 노드</th>
</tr>
</thead>
<tbody>
<tr>
<td style="width: 415px;" colspan="2">제조사 및 모델</td>
<td style="width: 269px;" colspan="2">Cray CS500</td>
</tr>
<tr>
<td style="width: 415px;" colspan="2">노드수</td>
<td style="width: 124px;">8,305</td>
<td style="width: 124px;">132</td>
</tr>
<tr>
<td style="width: 415px;" colspan="2">성능최고성능(Rpeak)</td>
<td style="width: 124px;">25.3 PFlops</td>
<td style="width: 124px;">0.4 PFlops</td>
</tr>
<tr>
<td style="width: 197px;" rowspan="5">프로세서</td>
<td style="width: 197px;">모델</td>
<td style="width: 124px;">Intel Xeon Phi 7250(KNL)</td>
<td style="width: 124px;">Intel Xeon 6148 (Skylake)</td>
</tr>
<tr>
<td style="width: 197px;">CPU당 이론성능</td>
<td style="width: 124px;">3.0464 TFLOPS</td>
<td style="width: 124px;">1.536 TFLOPS</td>
</tr>
<tr>
<td style="width: 197px;">CPU당 코어수</td>
<td style="width: 124px;">68</td>
<td style="width: 124px;">20</td>
</tr>
<tr>
<td style="width: 197px;">노드당 CPU수</td>
<td style="width: 124px;">1</td>
<td style="width: 124px;">2</td>
</tr>
<tr>
<td style="width: 197px;">On-package Memory</td>
<td style="width: 124px;">16GB, 490GB/s</td>
<td style="width: 124px;">-</td>
</tr>
<tr>
<td style="width: 197px;" rowspan="5">메인메모리</td>
<td style="width: 197px;">모델</td>
<td style="width: 124px;">16GB DDR4-2400</td>
<td style="width: 124px;">16GB DDR4-2666</td>
</tr>
<tr>
<td style="width: 197px;">구성</td>
<td style="width: 124px;">16GB x6, 6Ch per CPU</td>
<td style="width: 124px;">16GB x12, 6Ch per CPU</td>
</tr>
<tr>
<td style="width: 197px;">노드 당 메모리(GB)</td>
<td style="width: 124px;">96GB</td>
<td style="width: 124px;">192GB</td>
</tr>
<tr>
<td style="width: 197px;">CPU당 대역폭</td>
<td style="width: 124px;">115.2GB/s</td>
<td style="width: 124px;">128GB/s</td>
</tr>
<tr>
<td style="width: 197px;">전체크기</td>
<td style="width: 124px;">778.6TB</td>
<td style="width: 124px;">24.8TB</td>
</tr>
<tr>
<td style="width: 197px;" rowspan="4">고성능 인터커넥트</td>
<td style="width: 197px;">토폴로지</td>
<td style="width: 269px;" colspan="2">Fat Tree</td>
</tr>
<tr>
<td style="width: 197px;">Blocking Ratio</td>
<td style="width: 269px;" colspan="2">50% Blocking</td>
</tr>
<tr>
<td style="width: 197px;">Switches 구성</td>
<td style="width: 269px;" colspan="2">278x 48-port OPA edge switches<br>8x 768-port OPA core switches</td>
</tr>
<tr>
<td style="width: 197px;">포트 당 대역폭</td>
<td style="width: 269px;" colspan="2">100Gbps</td>
</tr>
<tr>
<td style="width: 197px;" rowspan="4">고성능 파일시스템(Burst Buffer)</td>
<td style="width: 197px;">서버수</td>
<td style="width: 269px;" colspan="2">DDN IME240 Server 48ea</td>
</tr>
<tr>
<td style="width: 197px;">서버 당 디스크</td>
<td style="width: 269px;" colspan="2">16x 1.2TB NVMe SSD, 2x 0.45TB NVMe SSD</td>
</tr>
<tr>
<td style="width: 197px;">전체 가용 용량</td>
<td style="width: 269px;" colspan="2">0.8PB</td>
</tr>
<tr>
<td style="width: 197px;">대역폭</td>
<td style="width: 269px;" colspan="2">20GB/sec per server, 0.8TB/s total</td>
</tr>
<tr>
<td style="width: 197px;" rowspan="4">병렬파일시스템</td>
<td style="width: 197px;">파일시스템</td>
<td style="width: 124px;" colspan="2">Lustre 2.7.21.3</td>
</tr>
<tr>
<td style="width: 197px;">전체 가용 용량</td>
<td style="width: 269px;" colspan="2">/scratch: 21PB, /home: 0.76PB, /apps: 0.5PB</td>
</tr>
<tr>
<td style="width: 197px;">대역폭</td>
<td style="width: 269px;" colspan="2">0.3TB/s</td>
</tr>
<tr>
<td style="width: 197px;">RAID 구성</td>
<td style="width: 269px;" colspan="2">RAID6(8D+2P)</td>
</tr>
</tbody>
</table>

![\[누리온 시스템 사양 및 구성\]](<../../../../.gitbook/assets/누리온 시스템 사양 및 구성.png>)
