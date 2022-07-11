# 누리온 VASP 6.1.0 설치 소개

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 vasp 6.1.0 Source 버전으로 설치 하는 방법에 대하여 소개 한다.

\


**1. 설치 환경**

|  **구분**     | **내용**                          |
| ----------- | ------------------------------- |
|  대상 시스템     |  누리온                            |
| OS Version  |  리눅스 / CentOS 7.7               |
|  CPU        |  Intel(R) Xeon Phi(TM) CPU 7250 |
|  컴파일러       |  Intel 2018.3 Version           |
|  MPI        |  IntelMPI 2018.3 Version        |
|  기타         |  Intel MKL Math Library         |

\


**2. 설치 전 환경 설정**

&#x20; 누리온 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여&#x20;

&#x20; 환경설정 툴인 Modules(http://modules.sourceforge.net)이 구성되어 있고,

&#x20; 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

\


\[ 환경 설정 ]

|  $ module load intel/18.0.3 impi/18.0.3 |
| --------------------------------------- |

\


**3. vasp 6.1.0 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.  \


|  **설치 과정**                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>$ tar xvzf vasp.6.1.0.tar.gz</p><p>$ cd vasp.6.1.0</p><p>$ vi makefile</p><p> - - - - - [ makefile 파일 수정 내용] 참고 - - - - - </p><p><br></p><p>$ cp arch/makefile.include.linux_intel makefile.include</p><p>$ vi makefile.include</p><p> - - - - - [ makefile.include 파일 수정 내용] 참고 - - - - - </p><p><br></p><p>$ make all</p> |

\


\


\[makefile 파일 수정 내용]

| <p>&#x3C;수정 전></p><p>VERSIONS = std gam ncl gpu gpu_ncl</p><p><br>&#x3C;수정 후>VERSIONS = std gam ncl</p> |
| ------------------------------------------------------------------------------------------------------- |

\


\


\[makefile.include 파일 수정 내용]

| <p>ADDITIONAL_OPTS=-O3 -qopenmp -xCOMMON-AVX512 -align array64byte</p><p>ADDITIONAL_OPTS_CC=-O3 -qopenmp -xCOMMON-AVX512</p><p><br></p><p># Precompiler options</p><p>CPP_OPTIONS= -DMPI -DHOST=\"IFC15_impi\" \</p><p>-DCACHE_SIZE=12000 -Davoidalloc \</p><p>-DMPI_BLOCK=8000  -Duse_collective \</p><p>-DnoAugXCmeta -Duse_bse_te \</p><p>-Dtbdyn \</p><p>-D_OPENMP -DPROFILING -Duse_shmem \</p><p>-Dshmem_bcast_buffer -Dsheme_rproj</p><p>CPP = fpp -f_com=no -free -w0 $*$(FUFFIX) $*$(SUFFIX) $(CPP_OPTIONS)</p><p>FC = mpiifort -mkl=parallel ${ADDITIONAL_OPTS}</p><p>FCL = mpiifort -mkl=parallel ${ADDITIONAL_OPTS}</p><p>FREE = -free -names lowercase</p><p>FFLAGS = -assume byterecl</p><p>OFLAG = ${ADDITIONAL_OPTS}</p><p>OFLAG_IN = $(OFLAG)</p><p>DEBUG = -O0</p><p>MKL_PATH = $(MKLROOT)/lib/intel64</p><p>BLAS =</p><p>LAPACK =</p><p>BLACS = </p><p>SCALAPACK = </p><p>OBJECTS = fftmpiw.o fftmpi_map.o fftw3d.o fft3dlib.o</p><p>INCS =-I$(MKLROOT)/include/fftw</p><p>LLIBS = $(SCALAPACK) $(LAPACK) $(BLAS)</p><p>OBJECTS_O1 += fft3dfurth.o fftw3d.o fftmpi.o fftmpiw.o</p><p>OBJECTS_O2 += fft3dlib.o</p><p><br></p><p># For what used to be vasp.5.lib</p><p>CPP_LIB = $(CPP)</p><p>FC_LIB = $(FC)</p><p>CC_LIB = icc</p><p>CFLAGS_LIB = -O</p><p>FFLAGS_LIB = ${ADDITIONAL_OPTS}</p><p>FREE_LIB = $(FREE)</p><p>OBJECTS_LIB= linpack_double.o getshmem.o</p><p><br></p><p># For the parser library</p><p>CXX_PARS = icpc</p><p>LIBS += parser</p><p>LLIBS += -Lparser -lparser -lstdc++</p><p><br></p><p># # Normally no need to change this</p><p>SRCDIR = ../../src</p><p>BINDIR = ../../bin</p><p><br></p><p>#================================================</p><p># GPU Stuff</p><p><br></p><p>CPP_GPU    = -DCUDA_GPU -DRPROMU_CPROJ_OVERLAP -DUSE_PINNED_MEMORY -DCUFFT_MIN=28 -UscaLAPACK</p><p><br></p><p>OBJECTS_GPU = fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d_gpu.o fftmpiw_gpu.o</p><p><br></p><p>CC         = icc</p><p>CXX        = icpc</p><p>CFLAGS     = -fPIC -DADD_ -Wall -openmp -DMAGMA_WITH_MKL -DMAGMA_SETAFFINITY -DGPUSHMEM=300 -DHAVE_CUBLAS</p><p><br></p><p>CUDA_ROOT  ?= /usr/local/cuda/</p><p>NVCC       := $(CUDA_ROOT)/bin/nvcc -ccbin=icc</p><p>CUDA_LIB   := -L$(CUDA_ROOT)/lib64 -lnvToolsExt -lcudart -lcuda -lcufft -lcublas</p><p><br></p><p>GENCODE_ARCH    := -gencode=arch=compute_30,code=\"sm_30,compute_30\" \</p><p>                   -gencode=arch=compute_35,code=\"sm_35,compute_35\" \</p><p>                   -gencode=arch=compute_60,code=\"sm_60,compute_60\"</p><p><br></p><p>MPI_INC    = $(I_MPI_ROOT)/include64/</p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\


\
