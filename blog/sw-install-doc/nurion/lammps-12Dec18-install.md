# 누리온 lammps-12Dec18 설치 소개

KISTI 슈퍼컴퓨터센터의 장비에 lammps-12Dec18 source 버전으로 설치 하는 방법에 대하여 소개 한다.

&#x20;

**1. 설치 환경**

|   **구분**    | **내용**                                 |
| ----------- | -------------------------------------- |
|  대상 시스템     | 누리온                                    |
|  OS Version | 리눅스 / CentOS 7.3                       |
|  CPU        | Intel(R) Xeon(R) Gold 6126             |
|  컴파일러       | Intel 2018.3 Version                   |
|  MPI        | <p>IntelMPI 2018.3 Version<br><br></p> |
|  기타         | Intel MKL Math Library                 |

&#x20;

**2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

**\[ 환경 설정 ]**

|  $ module load intel/18.0.3 impi/18.0.3 |
| --------------------------------------- |

\
**3. 설치 과정**

&#x20;설치 과정 소개는 tar를 이용한 압축 해제 방법과 설정 방법 등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. (다운로드URL : https://lammps.sandia.gov/tars/)\
\
설치 경로는 ${HOME}/lammps/12DEC18을 사용하였다. 이 위치는 사용자에게 맞는 위치로 변경하여야 한다. &#x20;

&#x20;**(1) VORO++ 설치** (다운로드 : http://math.lbl.gov/voro++/download/)

VORONOI 패키지 설치를 위한 voro++를 우선 설치한다.&#x20;

|   **설치과정**                                                                                                                                                                  |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ tar xvf voro++-0.4.6.tar.gz<br>$ cd voro++-0.4.6<br>$ mkdir -p ${HOME}/build/library<br>$ vi config.mk<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make<br>$ make install</p> |   |

&#x20;

\[config.mk 수정 사항]

&#x20;

| <p><strong>CXX=mpiicpc</strong><br><strong>CFLAGS= -Wall -ansi -pedantic -O3 -fPIC</strong><br>E_INC= -I../../src<br>E_LIB= -L../../src<br><strong>PREFIX= ${HOME}/build/library</strong><br>INSTALL= install<br>IFLAGS_EXEC= -m 0755<br>IFLAGS= -m 0644</p> |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | - |

&#x20;**(2) LATTE 설치** (다운로드 : https://github.com/lanl/LATTE/releases)

LATTTE 패키지 설치를 위한 Latte 라이브러리를 우선 설치한다.

다운로드 받은 파일을 적당한 위치($HOME/build)에 올린 후 다음과 같은 명령으로 압축 묶음 파일을 푼다.

|   **설치과정**                                                                                                                                        |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd ${HOME}/build<br>$ tar xzvf LATTE-1.2.1.tar.gz<br>$ cd LATTE-1.2.1<br>$ vi makefile.CHOICES<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make</p> |   |

&#x20;

\[makefile.CHOICES 수정 사항]

&#x20;

| <p>#<br># CPU Fortran options<br>#<br> <br>#For GNU compiler:<br>#FC = mpif90<br><strong>#FC = gfortran</strong><br><strong>#FCL = $(FC)</strong><br><strong>#FFLAGS = -O3 -fopenmp -cpp</strong><br>#FFLAGS = -fast -Mpreprocess -mp<br><strong>#LINKFLAG = -fopenmp</strong><br> <br>#For intel compiler:<br><strong>FC = ifort</strong><br><strong>FCL = $(FC)</strong><br><strong>FFLAGS = -O3 -fpp -qopenmp</strong><br><strong>LINKFLAG = -qopenmp</strong><br><strong>LIB = -mkl=parallel</strong><br> <br>#GNU BLAS/LAPACK libraries:<br><strong>#LIB = -llapack -lblas</strong><br> <br>#Intel MKL BLAS/LAPACK libraries:<br><strong>LIB = -Wl,--no-as-needed -L${MKLROOT}/lib/intel64 \</strong><br><strong>-lmkl_lapack95_lp64 -lmkl_gf_lp64 -lmkl_gnu_thread -lmkl_core \</strong><br><strong>-lmkl_gnu_thread -lmkl_core -ldl -lpthread -lm</strong><br> <br>#<br># GPU options<br>#<br> <br><strong>#GPU_CUDA_LIB = -L/opt/cudatoolkit-5.5/lib64 -lcublas -lcudart</strong><br><br><br><strong>#GPU_ARCH = sm_20</strong> </p> |   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20;**(3) 라이브러리 패키지 설치**

LAMMPS 홈페이지(http://lammps.sandia.gov/index.html)로부터 다운로드 받은 파일을 적당한 위치($HOME/build)에 올린 후 다음과 같은 명령으로 압축 묶음 파일을 푼다.

&#x20;

| $ tar xvf lammps-12Dec18.tar.gz |   |
| ------------------------------- | - |

&#x20;(3-1) voronoi 설치

(1)에서 설치한 voro++ 설치 디렉토리를 지정해 준다.

lammps 압축 해제후 lammps-12Dec18 폴더로 이동하여 아래의 작업을 진행한다.

|   **설치과정**                                                                                                                                                             |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lammps-12Dec18<br>$ cd lib/voronoi<br>$ ln -s ${HOME}/build/library/include/voro++ includelink<br>$ ln -s ${HOME}/build/library/lib liblink<br>$ cd ../../</p> |   |

&#x20; (3-2) poems 설치

|   **설치과정**                                                                                                          |   |
| ------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/poems<br>$ vi Makefile.mpi<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make -f Makefile.mpi<br>$ cd ../../</p> |   |

&#x20;

\[Makefile.mpi 수정 사항]

&#x20;

| <p><strong>CC =            mpiicpc</strong><br>CCFLAGS =       -O3 -g -fPIC -Wall #-Wno-deprecated<br>ARCHIVE =       ar<br>ARCHFLAG =      -rc<br>DEPFLAGS =      -M<br><strong>LINK =          mpiicpc</strong></p> |   |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20; (3-3) meam 설치

|   **설치과정**                                                                                                         |   |
| ------------------------------------------------------------------------------------------------------------------ | - |
| <p>$ cd lib/meam<br>$ vi Makefile.mpi<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make -f Makefile.mpi<br>$ cd ../../</p> |   |

&#x20;

\[Makefile.mpi 수정 사항]

&#x20;

| <p><strong>F90 =           mpiifort</strong><br><strong>CC  =           mpiicc</strong><br>F90FLAGS =      -O3 -fPIC<br>#F90FLAGS =      -O <br>ARCHIVE =       ar<br>ARCHFLAG =      -rc<br><strong>LINK =          mpiicpc</strong></p> |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20;(3-4) awpmd 설치

|   **설치과정**                                                                                                                                                                         |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/awpmd<br>$ vi Makefile.lammps.linalg<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ vi Makefile.mpi<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make -f Makefile.mpi<br>$ cd ../../</p> |   |

&#x20;

\[Makefile.lammps.installed 수정 사항]

&#x20;

| <p>user-atc_SYSINC =<br><strong>user-atc_SYSLIB = -llinalg</strong><br>user-atc_SYSPATH = -L../../lib/linalg$(LIBOBJDIR)</p> |   |
| ---------------------------------------------------------------------------------------------------------------------------- | - |

&#x20;

\[Makefile.mpi 수정 사항]

&#x20;

| **CC =        mpiicpc** |   |
| ----------------------- | - |

&#x20;

&#x20;(3-5) atc 설치

|   **설치과정**                                                                                                                                                                               |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/atc<br>$ vi Makefile.lammps.linalg<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ vi Makefile.mpi<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make -f Makefile.mpi<br><br><br>$ cd ../../</p> |   |

&#x20;

\[Makefile.lammps.linalg 수정 사항]

&#x20;

| <p>user-atc_SYSINC =<br><strong>user-atc_SYSLIB = -llinalg</strong><br>user-atc_SYSPATH = -L../../lib/linalg$(LIBOBJDIR)</p> |   |
| ---------------------------------------------------------------------------------------------------------------------------- | - |

\[Makefile.mpi 수정 사항]

&#x20;

| **CC =            mpiicpc** |   |
| --------------------------- | - |

&#x20;

&#x20;(3-6) linalg 설치

&#x20;

|   **설치과정**                                                                                                           |   |
| -------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/linalg<br>$ vi Makefile.mpi<br>----- 수정 사항은 아래의 내용 참고 -----<br>$ make -f Makefile.mpi<br>$ cd ../../</p> |   |

&#x20;

\[Makefile.mpi 수정 사항]

&#x20;

| <p><strong>FC = mpiifort</strong><br>FFLAGS = -O3 -fPIC<br>FFLAGS0 = -O0 -fPIC</p> |   |
| ---------------------------------------------------------------------------------- | - |

&#x20;(3-7) reax 설치

&#x20;

|   **설치과정**                                                      |   |
| --------------------------------------------------------------- | - |
| <p>$ cd lib/reax<br>$ make -f Makefile.ifort<br>$ cd ../../</p> |   |

&#x20;

&#x20;(3-8) latte 설치

&#x20;

|   **설치과정**                                                                                                                                                                                                                                                             |   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/latte<br>$ ln -s ${HOME}/build/LATTE-1.2.1/src includelink<br>$ ln -s ${HOME}/build/LATTE-1.2.1 liblink<br>$ ln -s ${HOME}/build/LATTE-1.2.1/src/latte_c_bind.o filelink.o<br>$ vi Makefile.lammps.mpi<br>----- 수정 사항은 아래의 내용 참고 ----- <br>$ cd ../../</p> |   |

&#x20;

\[Makefile.lammps.mpi 수정 사항]

&#x20;

| <p>latte_SYSINC =<br><strong>latte_SYSLIB = ../../lib/latte/filelink.o -llatte -llinalg -lifport</strong><br><strong>latte_SYSPATH = -L../../lib/linalg -qopenmp</strong></p> |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20;(3-9) message 설치

&#x20;

|   **설치과정**                                                                                                                                        |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>$ cd lib/message/cslib/src<br>$ vi Makefile<br>----- 수정 사항은 아래의 내용 참고 ----- <br>$ make lib_parallel zmq=no<br>$ cp libcsmpi.a libmessage.a</p> |   |

&#x20;

\[Makefile 수정 사항]

&#x20;

| <p>ifeq ($(MPI),YES)<br>  <strong>CC = mpiicpc</strong><br>else<br>  CCFLAGS += -I./STUBS_MPI<br>  LIB = libcsnompi.a<br>  SHLIB = libcsnompi.so<br>endif</p> |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20; **(4) LAMMPS 설치**

lammps 설치 디렉토리($HOME/build/lammps-12Dec18) 아래 src 폴더로 이동한다.

&#x20;

&#x20;\- package 선택 및 설치

사용하는 사용자의 연구내용에 맞추어 필요한 package를 선택하여 설치한다. 여기서는 기본적으로 많이 사용되는 package를 위주로 설치하였다.

&#x20;

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd src<br>$ make package-status<br>$ make yes-standard<br>$ make yes-message<br>$ make no-GPU<br>$ make no-PYTHON<br>$ make no-kim<br>$ make no-KOKKOS<br>$ make no-MSCG<br>$ make yes-USER-ATC<br>$ make yes-USER-AWPMD<br>$ make yes-USER-MEAMC<br>$ make yes-USER-OMP<br>$ make yes-USER-REAXC<br>$ make package-status<br>$ vi MAKE/Makefile.mpi<br>-- 수정 사항은 아래 내용 참고 --<br>$ vi Makefile.package.settings<br>-- 수정 사항은 아래 내용 참고 --<br><br><br>$ make mpi</p> | <p> <br> package 선택 확인<br> standard package 선택<br> <br> standard package 중 gpu package 제외<br> standard package 중 PYTHON package 제외<br> standard package 중 kim package 제외<br> standard package 중 KOKKOS package 제외<br> standard package 중 MSCG package 제외<br> <br> <br><br><br> <br> <br> package 선택 확인<br> <br> <br> <br> <br><br></p> |

&#x20;

\[MAKE/Makefile.mpi 수정 사항]

&#x20;

| <p><strong>CC = mpiicpc</strong><br><strong>OPTFLAGS = -xCOMMON-AVX512 -O3 -fp-model fast=2 -no-prec-div -qoverride-limits</strong><br><strong>CCFLAGS = -qopenmp -qno-offload -fno-alias -ansi-alias -restrict \</strong><br><strong>-DLMP_INTEL_USELRT -DLMP_USE_MKL_RNG $(OPTFLAGS)</strong><br><strong>CCFLAGS += -I/apps/compiler/intel/18.0.3/mkl/include -lmkl_rt</strong><br>SHFLAGS = -fPIC<br>DEPFLAGS = -M<br> <br><strong>LINK = mpiicpc</strong><br><strong>LINKFLAGS = -qopenmp $(OPTFLAGS)</strong><br>LIB =<br>SIZE = size<br> <br>ARCHIVE = ar<br>ARFLAGS = -rc<br>SHLIBFLAGS = -shared<br> <br><strong>FFT_INC = -DFFT_MKL -DFFT_SINGLE</strong><br>FFT_PATH =<br><strong>FFT_LIB = -L${MKLROOT}/lib/intel64/ -lmkl_intel_ilp64 -lmkl_sequential -lmkl_core</strong></p> |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | - |

&#x20;

\[Makefile.package.settings 수정 사항]

&#x20;

| <p>include ../../lib/awpmd/Makefile.lammps<br>include ../../lib/atc/Makefile.lammps<br><strong>include ../../lib/message/Makefile.lammps.nozmq</strong><br>include ../../lib/voronoi/Makefile.lammps<br>include ../../lib/reax/Makefile.lammps<br>include ../../lib/poems/Makefile.lammps<br>include ../../lib/meam/Makefile.lammps<br><strong>include ../../lib/latte/Makefile.lammps.mpi</strong><br>include ../../lib/compress/Makefile.lammps</p> |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |

**4. 실행 파일 복사**

&#x20;설치가 완료되면 사용에 편의를 위해 bin 경로를 만들어 실행 파일인 lmp\_mpi를 bin 경로에 복사한다.(선택사항)

&#x20;

| <p>$ ls -l lmp_mpi<br>$ cd ${HOME}/build/lammps-12Dec18/<br>$ mkdir bin<br>$ cp ${HOME}/build/lammps-12Dec18/src/lmp_mpi/bin .</p> |   |
| ---------------------------------------------------------------------------------------------------------------------------------- | - |

&#x20;

**5. 누리온에서 LAMMPS 사용을 위한 PBS 작업 스크립트 예제**

&#x20;위의 과정을 거처 설치된 lammps는 누리온 환경에서 다음과 같이 실행이 가능하다.

누리온에서 작업을 제출하기 위해서는 PBS 작업 스크립트를 사용하여야 한다.

&#x20;

&#x20;

실행 예제로는 examples/meam 아래의 데이터를 이용하였다.

&#x20;

|   **작업스크립트 예제**(lammps\_test-run.sh)                                                                                                                                                                                                                                                                                                                                          |   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - |
| <p>#!/bin/sh<br>#PBS -V<br>#PBS -N lammps_job_test<br>#PBS -q normal<br>#PBS -l select=2:ncpus=60:mpiprocs=60:ompthreads=1<br>#PBS -l walltime=04:00:00<br>#PBS -A lammps<br> <br>cd $PBS_O_WORKDIR<br> <br>module purge<br>module load intel/18.0.3 impi/18.0.3<br>export PATH=${HOME}/build/lammps-12Dec18/bin:$PATH<br> <br>mpirun lmp_mpi -in in.meamc<br> <br>exit 0</p> |   |

&#x20;

&#x20;
