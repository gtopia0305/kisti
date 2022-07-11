# 뉴론 VASP 5.4.4 설치 (GPU 버전)

KISTI 슈퍼컴퓨팅센터의 뉴론 시스템에 vasp 5.4.4 Source 버전으로 설치 하는 방법에 대하여 소개 합니다.

\


**1. 설치 환경**

|  **구분**     | **내용**                             |
| ----------- | ---------------------------------- |
|  대상 시스템     |  뉴론                                |
| OS Version  |  리눅스 / CentOS 7.4                  |
|  CPU        |  Intel Xeon E5-2670 v2             |
|  컴파일러       |  Intel 2018 Version                |
|  MPI        |  Mvapich2 2.3 Version              |
|  기타         |  Intel MKL Math Library, cuda 10.0 |

\


**2. 설치 전 환경 설정**

&#x20; 뉴론 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여&#x20;

&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,

&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용합니다.

\


\[ 환경 설정 ]

|  $ module load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 |
| ---------------------------------------------------------- |

\


**3. vasp 5.4.4 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략합니다.  \


|  **설치 과정**                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ tar xvzf vasp.5.4.4.tar.gz</p><p>$ cd vasp.5.4.4</p><p>$ cp arch/makefile.include.linux_intel ./makefile.include</p><p>$ vi makefile.include</p><p> - - - - - [ makefile.include 파일 수정 내용] 참고</p><p>$ make gpu</p> |

\


\[make.include 파일 수정 내용]

| <p># Precompiler options<br>CPP_OPTIONS= -DHOST=\"LinuxIFC\"\<br>             -DMPI -DMPI_BLOCK=8000 \<br>             -Duse_collective \<br>             -DCACHE_SIZE=16000 \<br>             -Davoidalloc \<br>             -Duse_bse_te \<br>             -Dtbdyn \<br>             -Duse_shmem</p><p><br></p><p>CPP        = fpp -f_com=no -free -w0  $*$(FUFFIX) $*$(SUFFIX) $(CPP_OPTIONS)</p><p><br></p><p>FC         = mpif90<br>FCL        = mpif90 -mkl -lstdc++</p><p><br></p><p>FREE       = -free -names lowercase</p><p><br></p><p>FFLAGS     = -assume byterecl -w<br>OFLAG      = -O2 -fPIC -xAVX<br>OFLAG_IN   = $(OFLAG)<br>DEBUG      = -O0</p><p>MKL_PATH   = $(MKLROOT)/lib/intel64<br>BLAS       =<br>LAPACK     =<br>OBJECTS    = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d.o</p><p>INCS       =-I$(MKLROOT)/include/fftw</p><p>LLIBS      = </p><p><br>OBJECTS_O1 += fftw3d.o fftmpi.o fftmpiw.o<br>OBJECTS_O2 += fft3dlib.o</p><p><br></p><p># For what used to be vasp.5.lib<br>CPP_LIB    = $(CPP)<br>FC_LIB     = $(FC)<br>CC_LIB     = icc<br>CFLAGS_LIB = -O<br>FFLAGS_LIB = -O1<br>FREE_LIB   = $(FREE)</p><p>OBJECTS_LIB= linpack_double.o getshmem.o</p><p><br></p><p># For the parser library<br>CXX_PARS   = icpc</p><p>LIBS       += parser<br>LLIBS      += -Lparser -lparser -lstdc++</p><p><br></p><p># Normally no need to change this<br>SRCDIR     = ../../src<br>BINDIR     = ../../bin</p><p><br></p><p>#================================================<br># GPU Stuff</p><p><br></p><p>CPP_GPU    = -DCUDA_GPU -DRPROMU_CPROJ_OVERLAP -DUSE_PINNED_MEMORY -DCUFFT_MIN=28 -UscaLAPACK</p><p>OBJECTS_GPU = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d_gpu.o fftmpiw_gpu.o</p><p>CC         = icc<br>CXX        = icpc<br>CFLAGS     = -fPIC -DADD_ -Wall -qopenmp -DMAGMA_WITH_MKL -DMAGMA_SETAFFINITY -DGPUSHMEM=300 -DHAVE_CUBLAS</p><p><br></p><p>CUDA_ROOT  ?= /apps/cuda/10.0<br>NVCC       := $(CUDA_ROOT)/bin/nvcc -ccbin=icc -std=c++11<br>CUDA_LIB   := -L$(CUDA_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas</p><p><br></p><p>GENCODE_ARCH    := -gencode=arch=compute_35,code=\"sm_35,compute_35\" \</p><p>-gencode=arch=compute_70,code=\"sm_70,compute_70\"</p><p><br></p><p>MPI_INC    = /apps/compiler/intel/18.0.2/cudampi/10.0/mvapich2/2.3/include</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\


**4. 기타**

vasp.5.4.4.18Apr17-6-g9f103f2a35 버전을 사용하는 경우 아래 참고 사이트를 확인하여 패치를 진행 해야 GPU 작업 오류가 발생하지 않습니다.

\


\- 참고 : [https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP](https://cms.mpi.univie.ac.at/wiki/index.php/Installing\_VASP)
