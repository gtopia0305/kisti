# 누리온 SIESTA 4.1-b3 설치(KNL)

KISTI 슈퍼컴퓨터센터의 누리온 시스템에 siesta-4.1-b3 source 버전으로 설치 하는 방법에 대하여 소개 한다.

&#x20;

**1. 설치 환경**

|   **구분**       | **내용**                                 |
| -------------- | -------------------------------------- |
|  대상 시스템        | 누리온                                    |
|  OS Version    | 리눅스 / CentOS 7.3                       |
|  CPU           | Intel(R) Xeon Phi(TM) CPU 7250         |
|  컴파일러          | Intel 2017.5 Version                   |
|  MPI           | <p>IntelMPI 2017.5 Version<br><br></p> |
| <p> 기타<br></p> | Intel MKL Math Library                 |

&#x20;

**2. 설치 전 환경 설정**

KISTI 시스템은 PATH, LD\_LIBRARY\_PATH 등을 쉽게 하기 위하여 OpenSource 인 Environment Modules(http://modules.sourceforge.net)이 구성되어 있고, 이하 설치 소개 에서는 module load를 이용한 환경 설정 방법을 이용한다.

**\[ 환경 설정 ]**

|  $ module load craype-mic-knl intel/17.0.5 impi/17.0.5 netcdf-hdf5-parallel/4.6.1 |
| --------------------------------------------------------------------------------- |

&#x20;

SIESTA 는 check 과정을 포함하고 있어 KNL CPU 타입 전용 옵션(-xMIC-AVX512) 사용을 위해서는 KNL 계산노드에서 빌드 해야 한다.

소개 문서는 KNL CPU 가 장착된 디버깅 노드에서 빌드 하는 방법으로 안내한다.

**\[ 디버깅 노드 접속 ]**

|  $ qsub -I -q debug -l select=1:ncpus=1:mpiprocs=1:ompthreads=1 -l walltime=12:00:00 -A siesta |
| ---------------------------------------------------------------------------------------------- |

\
**3. 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다. &#x20;

|   **설치과정**                                                                                                                                                                                                                   |             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| <p>$ tar -xvf siesta-4.1-b3.tar.gz<br>$ cd siesta-4.1-b3<br>$ cd Obj<br>$ sh ../Src/obj_setup.sh<br>$ cp intel.make arch.make<br>$ vi arch.make 수정<br> <strong></strong>   - - - [arch.make 파일 수정 내용] 참고 - - -<br>$ make</p> | <p><br></p> |

&#x20;

\[arch.make 파일 수정 내용]

| <p># <br># Copyright (C) 1996-2016 The SIESTA group<br>#  This file is distributed under the terms of the<br>#  GNU General Public License: see COPYING in the top directory<br>#  or http://www.gnu.org/copyleft/gpl.txt.<br># See Docs/Contributors.txt for a list of contributors.<br>#<br>#-------------------------------------------------------------------<br># arch.make file for gfortran compiler.<br># To use this arch.make file you should rename it to<br>#   arch.make<br># or make a sym-link.<br># For an explanation of the flags see DOCUMENTED-TEMPLATE.make<br><br><br>.SUFFIXES:<br>.SUFFIXES: .f .F .o .c .a .f90 .F90<br><br><br>SIESTA_ARCH = unknown<br><br><br>CC = mpiicc<br>FPP = $(FC) -E -P<br>FC = mpiifort<br>FC_SERIAL = ifort<br><br><br>FFLAGS = -O2 -fPIC<br><br><br>AR = ar<br>RANLIB = ranlib<br><br><br>SYS = nag<br><br><br>SP_KIND = 4<br>DP_KIND = 8<br>KINDS = $(SP_KIND) $(DP_KIND)<br><br><br>LDFLAGS = -mkl=cluster<br><br><br>COMP_LIBS = libsiestaLAPACK.a libsiestaBLAS.a<br><br><br>FPPFLAGS = $(DEFS_PREFIX)-DFC_HAVE_ABORT<br><br><br>MPI_INTERFACE = libmpi_f90.a<br><br><br>MPI_INCLUDE = .<br><br><br>FPPFLAGS += -DMPI<br><br><br>LIBS = -mkl=cluster -L/apps/compiler/intel/17.0.5/impi/17.0.5/applib2/mic-knl/netcdf-hdf5-parallel/4.6.1/lib -lnetcdf<br><br><br># Dependency rules ---------<br><br><br>FFLAGS_DEBUG = -g -O1   # your appropriate flags here...<br><br><br># The atom.f code is very vulnerable. Particularly the Intel compiler<br># will make an erroneous compilation of atom.f with high optimization<br># levels.<br>atom.o: atom.F<br>$(FC) -c $(FFLAGS_DEBUG) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $&#x3C; <br>pdos2k.o: pdos2k.F<br>$(FC) -c $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $&#x3C;<br>pdos3k.o: pdos3k.F<br>$(FC) -c $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F) $&#x3C;<br><br><br>.c.o:<br>$(CC) -c $(CFLAGS) $(INCFLAGS) $(CPPFLAGS) $&#x3C; <br>.F.o:<br>$(FC) -c $(FFLAGS) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_fixed_F)  $&#x3C; <br>.F90.o:<br>$(FC) -c $(FFLAGS) $(INCFLAGS) $(FPPFLAGS) $(FPPFLAGS_free_F90) $&#x3C; <br>.f.o:<br>$(FC) -c $(FFLAGS) $(INCFLAGS) $(FCFLAGS_fixed_f)  $&#x3C;<br>.f90.o:<br>$(FC) -c $(FFLAGS) $(INCFLAGS) $(FCFLAGS_free_f90)  $&#x3C;<br><br></p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

&#x20;
