---
description: 슈퍼컴퓨팅인프라센터 2019. 10. 25. 15:11
---

# 누리온 OpenFOAM-7 버전 설치 소개(KNL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 OpenFOAM-v7 Source 버전으로 설치하는 과정을 정리하여 사용자들이 설치하는 방법에 대하여 참고할 수 있도록 내용을 소개한다.



**1. 설치 환경**

|   **구분**    | **내용**                         |
| ----------- | ------------------------------ |
|  대상 시스템     | 누리온                            |
|  OS Version | 리눅스 / CentOS 7.3               |
|  CPU        | Intel(R) Xeon Phi(TM) CPU 7250 |
|  컴파일러       | Intel 2018.3                   |
|  MPI        | IntelMPI 2018.3                |
|  기타         |                                |



**2. 설치 전 환경 설정**

OpenFOAM-v7 버전 설치에 필요한 gmp, mpfr, mpc, boost, CGAL은 누리온 시스템에 미리 설치된 /apps/commons 라이브러리들을 사용한다.\
만약 다른 버전의 gmp, mpfr, mpc, boost, CGAL이 필요한 경우는 사용자의 홈 디렉터리(/home01/$USER)에 설치 후 환경 설정을 해서 사용하면 된다.



**\[ 환경 설정 ]**

| <p>$ module load cmake/3.12.3<br>$ module load intel/18.0.3 impi/18.0.3</p> |
| --------------------------------------------------------------------------- |

\
**3. OpenFOAM-v7 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고, 소스 파일 다운로드 등은 생략한다.   설치 소개 시 사용된 경로 <mark style="color:blue;">/Install\_Path/KNL</mark>는 설치 안내를 위한 경로이므로, 사용자는 실제 사용할 경로를 지정하여 설치하면 된다. &#x20;

|   **설치과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>$ cd <mark style="color:blue;">/Install_Path/KNL</mark><br>$ tar xzvf OpenFOAM-7-20190902.tar.gz<br>$ tar xzvf ThirdParty-7-version-7.tar.gz<br>$ mv OpenFOAM-7-20190902/ OpenFOAM-7<br>$ mv ThirdParty-7-version-7/ ThirdParty-7<br>$ vi OpenFOAM-7/etc/config.sh/settings <br><em><mark style="color:orange;"><strong>- - - [settings 수정 사항] 참고 - - -</strong></mark></em><br>$ vi OpenFOAM-7/etc/bashrc <br><em><mark style="color:orange;"><strong>- - - [bashrc 수정 사항] 참고 - - -</strong></mark></em><br>$ vi ThirdParty-7/makeCGAL<br> <em><mark style="color:orange;"><strong>- - - [makeCGAL 수정 사항] 참고 - - -</strong></mark></em><br>$ vi OpenFOAM-7/etc/config.sh/mpi <br><em><mark style="color:orange;"><strong>- - - [mpi 수정 사항] 참고 - - -</strong></mark></em><br>$ vi OpenFOAM-7/wmake/rules/linux64Icc/c++ <br><em><mark style="color:orange;"><strong>- - - [c++ 수정 사항] 참고 - - -</strong></mark></em><br>$ vi ThirdParty-7/scotch_6.0.6/src/Makefile.inc<br> <mark style="color:orange;">  </mark><em><mark style="color:orange;"> <strong>- - - [Makefile.inc 수정 사항] 참고 - - -</strong></mark></em><br>$ sed -i -e 's/\(boost_version=\)boost-system/\1boost_1_68_0/' OpenFOAM-7/etc/config.sh/CGAL<br>$ sed -i -e 's/\(cgal_version=\)cgal-system/\1CGAL-4.9.1/' OpenFOAM-7/etc/config.sh/CGAL<br>$ source OpenFOAM-7/etc/bashrc<br>$ mkdir -p $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER<br>$ ln -s /apps/common/gmp/6.1.2 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/gmp-system<br>$ ln -s /apps/common/mpfr/4.0.1 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpfr-system<br>$ ln -s /apps/common/mpc/1.1.0 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpc-system<br>$ ln -s /apps/common/boost/1.68.0 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/boost_1_68_0<br>$ ln -s /apps/common/CGAL/4.9.1 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/CGAL-4.9.1<br>$ ln -s /apps/applications/cmake/3.12.3 $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/cmake-system<br><br>$ cd ThirdParty-7<br>$ ./Allwmake<br><br>$ cd $WM_PROJECT_DIR<br>$ ./Allwmake </p> |



**\[settings 수정 사항]**

|  수정 전 | <p>64)<br>            WM_ARCH=linux64<br>            export WM_COMPILER_LIB_ARCH=64<br>            export WM_CC='gcc'<br>            export WM_CXX='g++'<br>            export WM_CFLAGS='-m64 -fPIC'<br>            export WM_CXXFLAGS='-m64 -fPIC -std=c++0x'<br><br>            export WM_LDFLAGS='-m64'</p>                                                                                           |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  수정 후 | <p> 64)<br>            WM_ARCH=linux64<br>            export WM_COMPILER_LIB_ARCH=64<br>            export WM_CC='icc'<br>            export WM_CXX='icpc'<br>            export WM_CFLAGS='-m64 -fPIC <mark style="color:blue;">-xMIC-AVX512</mark>'<br>            export WM_CXXFLAGS='-m64 -fPIC <mark style="color:blue;">-xMIC-AVX512</mark> -std=c++0x'<br>            export WM_LDFLAGS='-m64'</p> |

※ <mark style="color:blue;">"-xMIC-AVX512"</mark> 옵션은 <mark style="color:blue;">KNL CPU 타입 전용 옵션</mark>으로, SKL 계산노드와 KNL 계산노드 모두 실행을 희망하는 경우는 "<mark style="color:blue;">-xCOMMON-AVX512</mark>" 로 변경해서 사용



**\[bashrc 수정 사항]**

|  수정 전 | <p> [ "$BASH" -o "$ZSH_NAME" ] &#x26;&#x26; \<br> export FOAM_INST_DIR=$(cd $(dirname $)/../.. &#x26;&#x26; pwd -P) || \<br> export FOAM_INST_DIR=$HOME/$WM_PROJECT<br> <br> export WM_COMPILER=Gcc<br> export WM_MPLIB=SYSTEMOPENMPI</p> |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  수정 후 | <p> export FOAM_INST_DIR=<mark style="color:blue;">/Install_Path/KNL</mark><br><br><br> export WM_COMPILER=Icc        <em>(I는 대문자 cc는 소문자)</em><br> export WM_MPLIB=INTELMPI</p>                                                          |

※ 위 수정 예제에서 **** <mark style="color:blue;">/Install\_Path/KNL</mark> 경로는 설치 시 사용된 경로이므로 변경 후 사용



**\[makeCGAL] 수정 사항]**

| 수정 전  | <p> cgalPACKAGE=${cgal_version:-CGAL-4.10}<br> boostPACKAGE=${boost_version:-boost-system}</p>  |
| ----- | ----------------------------------------------------------------------------------------------- |
| 수정 후  | <p> cgalPACKAGE=${cgal_version:-CGAL-4.9.1}<br> boostPACKAGE=${boost_version:-boost_1_68_0}</p> |



**\[mpi 수정 사항]**

|  수정 전 |  libDir=\`mpicc --showme:link \| sed -e 's/.\*-L\\(\[^ ]\*\\).\*/\1/'\`  |
| ----- | ------------------------------------------------------------------------ |
|  수정 후 |  libDir=\`mpiicc --showme:link \| sed -e 's/.\*-L\\(\[^ ]\*\\).\*/\1/'\` |



**\[c++ 수정 사항]**

|  수정 전 |  CC          = icpc -std=c++11 -fp-trap=common -fp-model precise                                              |
| ----- | ------------------------------------------------------------------------------------------------------------- |
|  수정 후 |  CC         = icpc -std=c++11 -<mark style="color:blue;">xMIC-AVX512</mark> -fp-trap=common -fp-model precise |



**\[Makefile.inc 수정 사항]**

|  수정 전 | <p> AR              = gcc<br> ARFLAGS         = $(WM_CFLAGS) -shared -o<br> CCS             = gcc<br> CCP             = mpicc<br> CCD             = mpicc<br> CFLAGS          = $(WM_CFLAGS) -O3 -DCOMMON_FILE_COMPRESS_GZ -DCOMMON_RANDOM_FIXED_SEED - DSCOTCH_RENAME -Drestrict=__restrict<br> CLIBFLAGS       = -shared</p>                               |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  수정 후 | <p> AR              = icc<br> ARFLAGS         = -m64 -fPIC -shared -o<br> CCS             = icc<br> CCP             = mpiicc<br> CCD             = mpiicc<br> CFLAGS          = -O3 -DCOMMON_FILE_COMPRESS_GZ -DCOMMON_RANDOM_FIXED_SEED - DSCOTCH_RENAME -Drestrict=__restrict -xCOMMON-AVX512<br> <br> CLIBFLAGS       = -shared -fPIC -xCOMMON-AVX512</p> |



**4. 테스트**

| <p>$ module load intel/18.0.3 impi/18.0.3<br>$ source /Install_Path/KNL/OpenFOAM-7/etc/bashrc <br>$ mkdir -p $FOAM_RUN <br>$ run <br>$ cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .<br>$ cd pitzDaily <br>$ blockMesh <br>$ simpleFoam </p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**5. 기타**

Intel 컴파일러를 이용하여 OpenFOAM 설치 시 KNL CPU 타입 전용 옵션인 "-xMIC-AVX512"를 사용하게 될 경우 KNL 계산노드에서 설치를 진행해야 오류가 발생되지 않는다.\
"-xMIC-AVX512" 옵션을 사용할 경우 PBS 스케줄러의 Interactive 기능을 이용하여 KNL 계산노드로 접속하여 빌드를 진행해야 한다.\
아래는 누리온 KNL 계산노드(debug 큐) 로 접속하는 예제이다.

|  $ qsub -I -V -q debug -l select=1:ncpus=68:mpiprocs=68:ompthreads=1 -l walltime=12:00:00 -A openfoam |
| ----------------------------------------------------------------------------------------------------- |

**※ qsub 명령 뒤 문자는 I(대문자 아이)  이고, select 와 walltime 앞에 문자는 l(소문자 엘) 이다.**
