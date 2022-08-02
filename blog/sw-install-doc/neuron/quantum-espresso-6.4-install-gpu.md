---
description: 슈퍼컴퓨팅인프라센터 2019. 7. 30. 10:12
---

# 뉴론 Quantum Espresso-6.4 (GPU 버전) 설치

KISTI 슈퍼컴퓨터센터의 장비에 qe-6.4 source 버전으로 설치하는 방법에 대하여 소개한다.

&#x20;

**1. 설치 환경**

|   **구분**       | **내용**                |
| -------------- | --------------------- |
|  대상 시스템        | 뉴론                    |
|  OS Version    | 리눅스 / CentOS 7.4      |
|  CPU           | Intel Xeon E5-2670 v2 |
|  컴파일러          | PGI 2019 Version      |
|  MPI           | Openmpi 3.1.0 Version |
| <p> 기타<br></p> | CUDA 10.0             |

&#x20;

**2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

****

**\[ 환경 설정 ]**

|  $ module load pgi/19.1 cuda/10.0 cudampi/openmpi-3.1.0 |
| ------------------------------------------------------- |



**3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. (다운로드 : https://gitlab.com/QEF/q-e-gpu/-/releases)

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvf q-e-gpu-qe-gpu-6.4a1.tar.gz<br><br>$ cd q-e-gpu-qe-gpu-6.4a1<br><br>$ export F90FLAGS="-fast -Ktrap=fp -Mcache_align -Mpreprocess -Mlarge_arrays -mp -tp=px"<br><br>$ export FFLAGS="-fast -Ktrap=fp -mp -tp=px"<br><br>$ ./configure --prefix=$/QE/6.4 \<br><br>--with-cuda=/apps/cuda/10.0 --with-cuda-cc=70 --with-cuda-runtime=10.0 --enable-openmp<br><br>$ vi make.inc<br><br>-----아래 내용으로 수정 -----<br>CFLAGS         = -fast -tp=px -Mpreprocess $(DFLAGS) $(IFLAGS)<br>FOX_FLAGS = -fast -tp=px -Mcache_align -Mpreprocess -Mlarge_arrays<br><br>$ make pw</p> |

&#x20;&#x20;

4\. 뉴론에서 QE 사용을 위한 SLURM 작업 스크립트 예제

&#x20;위의 과정을 거처 설치된 QE는 뉴론 환경에서 다음과 같이 실행이 가능하다.\
**** 뉴론에서 작업을 제출하기 위해서는 SLURM 작업 스크립트를 사용하여야 한다.\
&#x20;(작업 스크립트 예제 경로: /apps/applications/test\_samples/QE/)

|   ****  작업스크립트 예제(qe\_job.sh)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>#!/bin/sh<br><br>#SBATCH -J "QE-GPU"<br><br>#SBATCH -p cas_v100_2<br>#SBATCH -N 1<br>#SBATCH -n 20<br>#SBATCH -o %x_%j.out <br><br>#SBATCH -e %x_%j.err<br><br>#SBATCH -t 00:30:00<br><br>#SBATCH --gres=gpu:2<br><br>#SBATCH --comment qe<br> <br>ulimit -a unlimited<br> <br>export NO_STOP_MESSAGE=yes<br> <br>## Setting for gpu acceleration off<br><br>#### export USEGPU=no<br> <br># QE run parameters<br>NGPU=2<br><br>NPOOL=1<br> <br># Node-specific parameters<br>GPU_PER_SOCKET=1<br><br>CORES_PER_SOCKET=10<br> <br>NCORE_PER_RANK=$(($/$))<br> <br>export OMP_NUM_THREADS=$<br><br>export MKL_NUM_THREADS=$<br> <br>srun --cpu_bind=none -n $ -c $ pw.x -input ./pw.in -npool $</p> |

&#x20;
