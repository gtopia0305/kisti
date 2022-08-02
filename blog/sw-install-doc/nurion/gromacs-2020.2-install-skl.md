---
description: 슈퍼컴퓨팅인프라센터 2020. 6. 26. 13:44
---

# 누리온 GROMACS-2020.2 버전 설치 소개 (SKL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 gromacs-2020.2 Source 버전으로 설치 하는 방법에 대하여 소개 한다.



**1. 설치 환경**

|  **구분**      | **내용**                      |
| ------------ | --------------------------- |
|  대상 시스템      |  누리온                        |
|  OS Version  |  리눅스 / CentOS 7.7           |
|  CPU         |  Intel(R) Xeon(R) Gold 6148 |
|  컴파일러        |  Intel 2019.5 Version       |
|  MPI         |  IntelMPI 2019.5 Version    |
|  기타          |  Intel MKL Math Library     |



**2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여\
&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,\
&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.



\[ 환경 설정 ]

| <p> $ module load intel/19.0.5 impi/19.0.5 cmake/3.12.3</p><p> $ export LD_LIBRARY_PATH=/apps/compiler/gcc/7.2.0/lib64:$LD_LIBRARY_PATH</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------- |

\


**3. gromacs-2020.2 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ export FLAGS="-O2 -xCORE-AVX512 -g -static-intel"</p><p> $ export CFLAGS=$FLAGS </p><p> $ export CXXFLAGS=$FLAGS </p><p> $ export CC=mpiicc </p><p> $ export CXX=mpiicpc </p><p> $ export CPATH=/apps/compiler/intel/19.0.5/mkl/include:$CPATH</p><p><br></p><p> $ tar xvzf gromacs-2020.2.tar.gz</p><p> $ cd gromacs-2020.2</p><p> $ <strong>mkdir build</strong></p><p> $ cd build</p><p><br></p><p> $ cmake .. <mark style="color:blue;"><strong>-DCMAKE_INSTALL_PREFIX=${HOME}/GROMACS/2020.2</strong></mark> \</p><p>-DGMX_FFT_LIBRARY=mkl -DGMX_MPI=ON -DGMX_OPENMP=ON \</p><p>-DGMX_CYCLE_SUBCOUNTERS=ON -DGMX_GPU=OFF \</p><p>-DGMX_BUILD_HELP=OFF -DGMX_HWLOC=OFF \</p><p>-DGMX_SIMD=AVX_512 -DGMX_OPENMP_MAX_THREADS=32 \</p><p>-DGMX_GPLUSPLUS_PATH=/apps/compiler/gcc/7.2.0/bin/g++</p><p><br></p><p> $ make</p><p> $ make install</p> |

※ <mark style="color:blue;">**"-DCMAKE\_INSTALL\_PREFIX=${HOME}/GROMACS/2020.2**</mark><mark style="color:blue;">"</mark>는 예시로 설치 희망하는 디렉토리로 명시한다.
