# 이슈 테스트

테이블 이

| 구분                      | KNL 계산노드                                                               | Cpu-only 노드              |                           |
| ----------------------- | ---------------------------------------------------------------------- | ------------------------ | ------------------------- |
| 제조사 및 모델                | Cray CS500                                                             |                          |                           |
| 노드수                     | 8,305                                                                  | 132                      |                           |
| 성능최고성능(Rpeak)           | 25.3 PFlops                                                            | 0.4 PFlops               |                           |
| 프로세서                    | 모델                                                                     | Intel Xeon Phi 7250(KNL) | Intel Xeon 6148 (Skylake) |
| CPU당 이론성능               | 3.0464 TFLOPS                                                          | 1.536 TFLOPS             |                           |
| CPU당 코어수                | 68                                                                     | 20                       |                           |
| 노드당 CPU수                | 1                                                                      | 2                        |                           |
| On-package Memory       | 16GB, 490GB/s                                                          | -                        |                           |
| 메인메모리                   | 모델                                                                     | 16GB DDR4-2400           | 16GB DDR4-2666            |
| 구성                      | 16GB x6, 6Ch per CPU                                                   | 16GB x12, 6Ch per CPU    |                           |
| 노드 당 메모리(GB)            | 96GB                                                                   | 192GB                    |                           |
| CPU당 대역폭                | 115.2GB/s                                                              | 128GB/s                  |                           |
| 전체크기                    | 778.6TB                                                                | 24.8TB                   |                           |
| 고성능 인터커넥트               | 토폴로지                                                                   | Fat Tree                 |                           |
| Blocking Ratio          | 50% Blocking                                                           |                          |                           |
| Switches 구성             | <p>278x 48-port OPA edge switches<br>8x 768-port OPA core switches</p> |                          |                           |
| 포트 당 대역폭                | 100Gbps                                                                |                          |                           |
| 고성능 파일시스템(Burst Buffer) | 서버수                                                                    | DDN IME240 Server 48ea   |                           |
| 서버 당 디스크                | 16x 1.2TB NVMe SSD, 2x 0.45TB NVMe SSD                                 |                          |                           |
| 전체 가용 용량                | 0.8PB                                                                  |                          |                           |
| 대역폭                     | 20GB/sec per server, 0.8TB/s total                                     |                          |                           |
| 병렬파일시스템                 | 파일시스템                                                                  | Lustre 2.7.21.3          |                           |
| 전체 가용 용량                | /scratch: 21PB, /home: 0.76PB, /apps: 0.5PB                            |                          |                           |
| 대역폭                     | 0.3TB/s                                                                |                          |                           |
| RAID 구성                 | RAID6(8D+2P)                                                           |                          |                           |

코드블럭 이슈 - 내부 텍스트 색상변환 안

```
#!/bin/sh#PBS -V
#PBS -N <mark style="color:blue;">Nastran_job</mark>
#PBS -q comrcial
#PBS -l select=1i:ncpus=40:mpiprocs=1:ompthreads=40#PBS -l walltime=04:00:00#PBS -A nastrancd $PBS_O_WORKDIR/apps/commercial/MSC/Nastran/bin/nast20182 car_mod_freq.bdf smp=$NCPUS batch=no sdir="."
```

<mark style="color:red;">텍스트 색상 변환 레드</mark>

텍스트 색상 변환 블루
