---
description: 슈퍼컴퓨팅인프라센터 2019. 6. 27. 11:33
---

# 누리온 OpenFOAM-5.x 버전 설치 소개(SKL)

KISTI 슈퍼컴퓨팅센터의 누리온 시스템에 OpenFOAM-v5.0 Source 버전으로 공용 파일시스템인 /apps에 설치했던 내용을 정리하여 사용자들이 설치하는 방법에 대하여 참고할 수 있도록 내용을 소개 한다.



**1. 설치 환경**

|  **구분**     | **내용**                      |
| ----------- | --------------------------- |
|  대상 시스템     |  누리온                        |
| OS Version  |  리눅스 / CentOS 7.3           |
|  CPU        |  Intel(R) Xeon(R) Gold 6126 |
|  컴파일러       |  Intel 2018.3               |
|  MPI        |  IntelMPI 2018.3            |
|  기타         |                             |



**2. 설치 전 환경 설정**

&#x20;OpenFOAM-v5.x 버전 설치에 필요한 gmp, mpfr, mpc, boost, CGAL 는 누리온 시스템에 미리 설치된 /apps/common 라이브러리들을 사용한다.\
&#x20;만약 다른 버전의 gmp, mpfr, mpc, boost, CGAL  가 필요한 경우는 사용자의 홈 디렉토리(/home01/$USER)에 설치 후 환경설정을 해서 사용하면 된다.



\[ 환경 설정 ]

| <p> $ module load cmake/3.12.3</p><p> $ module load intel/18.0.3 impi/18.0.3</p> |
| -------------------------------------------------------------------------------- |



**3. OpenFOAM-v5.0 버전 설치 과정**

&#x20;설치 과정 소개는 tar 를 이용한 압축 해제 방법과 설정 방법등 진행 절차를 위주로 설명하고,

&#x20;소스 파일 다운로드 등은 생략한다.   설치 소개 시 사용된 경로 <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL/</mark> 는 설치 안내를 위한 경로이므로, 사용자는 실제 사용할 경로를 지정하여 설치하면 된다. \


|  **설치 과정**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p> $ cd <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL/</mark></p><p> $ mkdir OpenFOAM</p><p> $ cd OpenFOAM</p><p> $ tar -xzf OpenFOAM-5.x-version-5.0.tar.gz </p><p> $ tar -xzf ThirdParty-5.x-version-5.0.tar.gz</p><p> $ mv OpenFOAM-5.x-version-5.0 OpenFOAM-5.0</p><p> $ mv ThirdParty-5.x-version-5.0 ThirdParty-5.0</p><p> $ vi OpenFOAM-5.0/etc/config.sh/settings </p><p> <strong>  </strong><em><strong>-</strong><strong> </strong><mark style="color:orange;"><strong>- - [settings 수정 사항] 참고 - - -</strong></mark></em></p><p> $ vi OpenFOAM-5.0/etc/bashrc </p><p>  <strong>  </strong><em><mark style="color:orange;"><strong>- - - [bashrc 수정 사항] 참고 - - -</strong></mark></em></p><p> $ vi ThirdParty-5.0/makeCGAL</p><p> <strong>  </strong><em><strong></strong><strong> </strong><mark style="color:orange;"><strong>- - - [makeCGAL 수정 사항] 참고 - - -</strong></mark></em></p><p> $ vi OpenFOAM-5.0/etc/config.sh/mpi</p><p> <strong>  </strong><em><mark style="color:orange;"><strong>- - - [mpi 수정 사항] 참고 - - -</strong></mark></em></p><p> $ vi OpenFOAM-5.0/wmake/rules/linux64Icc/c++ </p><p> <strong>  </strong><em><strong> </strong><mark style="color:orange;"><strong>- - - [c++ 수정 사항] 참고 - - -</strong></mark></em><mark style="color:orange;"> </mark> </p><p>$ vi ThirdParty-5.0/scotch_6.0.3/src/Make.inc/Makefile.inc.x86-64_pc_linux2.icc</p><p> <em> <mark style="color:orange;"><strong>- - - [Makefile.inc.x86-64_pc_linux2.icc 수정 사항] 참고 - - -</strong></mark></em></p><p> $ vi ThirdParty-5.0/etc/wmakeFiles/scotch/Makefile.inc.i686_pc_linux2.shlib-OpenFOAM</p><p> <em> <mark style="color:orange;"><strong>- - - [Makefile.inc.i686_pc_linux2.shlib-OpenFOAM 수정 사항] 참고 - - -</strong></mark></em></p><p> $ sed -i -e 's/\(boost_version=\)boost-system/\1boost_1_68_0/' OpenFOAM-5.0/etc/config.sh/CGAL</p><p> $ sed -i -e 's/\(cgal_version=\)cgal-system/\1CGAL-4.9.1/' OpenFOAM-5.0/etc/config.sh/CGAL</p><p> $ source OpenFOAM-5.0/etc/bashrc </p><p> $ mkdir -p $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER</p><p> $ ln -s /apps/common/gmp/6.1.2          $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/gmp-system</p><p> $ ln -s /apps/common/mpfr/4.0.1         $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpfr-system</p><p> $ ln -s /apps/common/mpc/1.1.0          $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/mpc-system</p><p> $ ln -s /apps/common/boost/1.68.0       $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/boost_1_68_0</p><p> $ ln -s /apps/common/CGAL/4.9.1         $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/CGAL-4.9.1</p><p> $ ln -s /apps/applications/cmake/3.12.3  $WM_THIRD_PARTY_DIR/platforms/$WM_ARCH$WM_COMPILER/cmake-system</p><p><br></p><p> $ cd ThirdParty-5.0</p><p> $ ./Allwmake</p><p><br> $ cd $WM_PROJECT_DIR</p><p> $ ./Allwmake</p> |



**\[settings 수정 사항]**&#x20;

| <p> 38line :      WM_ARCH=linux64</p><p> 61line :         64)</p><p> 62line :             WM_ARCH=linux64</p><p> 63line :             export WM_COMPILER_LIB_ARCH=64</p><p> 64line :             export WM_CC='icc'</p><p> 65line :             export WM_CXX='icpc'</p><p> 66line :             export WM_CFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark>'</p><p> 67line :             export WM_CXXFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark> -std=c++0x'</p><p> 68line :             export WM_LDFLAGS='-m64 -fPIC <mark style="color:blue;">-xCORE-AVX512</mark>'</p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ "<mark style="color:blue;">-xCORE-AVX512</mark>" 옵션은 <mark style="color:blue;">SKL CPU 타입 전용 옵션</mark>으로, \
&#x20;SKL 계산노드와 KNL 계산노드 모두 실행을 희망하는 경우는 "-<mark style="color:blue;">xCOMMON-AVX512</mark>" 로 변경해서 사용



**\[bashrc 수정 사항]**&#x20;

| <p> 47line : #export FOAM_INST_DIR=$HOME/$WM_PROJECT</p><p> 50line : export FOAM_INST_DIR=<mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL</mark>/$WM_PROJECT</p><p> 65line : export WM_COMPILER=Icc </p><p> 89line : export WM_MPLIB=INTELMPI</p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

※ 위 수정 예제에서 **** <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL</mark> **** 경로는 설치 시 사용된 경로이므로 변경 후 사용



**\[makeCGAL 수정 사항]**&#x20;

| <p> 46line : cgalPACKAGE=${cgal_version:-CGAL-4.8.1}</p><p> 47line : boostPACKAGE=${boost_version:-boost_1_68_0}</p> |
| -------------------------------------------------------------------------------------------------------------------- |



**\[mpi 수정 사항]**&#x20;

|  46line :     libDir=\`mpiicc --showme:link \| sed -e 's/.\*-L\\(\[^ ]\*\\).\*/\1/'\` |
| ------------------------------------------------------------------------------------- |



**\[c++ 수정 사항]**&#x20;

|  9line : CC          = icpc -std=c++11 -<mark style="color:blue;">xCORE-AVX512</mark> -fp-trap=common -fp-model precise |
| ----------------------------------------------------------------------------------------------------------------------- |



**\[Makefile.inc.x86-64\_pc\_linux2.icc 수정 사항]**&#x20;

| <p> 10line : CCP             = mpiicc</p><p> 12line : CFLAGS          = $(WM_CFLAGS) -O3 -DCOMMON_FILE_COMPRESS_GZ -DCOMMON_PTHREAD -DCOMMON_RANDOM_FIXED_SEED -DSCOTCH_RENAME -DSCOTCH_PTHREAD -restrict -DIDXSIZE64</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**\[Makefile.inc.i686\_pc\_linux2.shlib-OpenFOAM 수정 사항]**&#x20;

| <p>  6line : AR              = icc</p><p>  9line : CCS             = icc</p><p> 10line : CCP             = mpiicc</p><p> 11line : CCD             = mpiicc</p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- |



**4. 테스트**

| <p> $ module load intel/18.0.3 impi/18.0.3</p><p> $ source <mark style="color:blue;">${HOME}/OpenFOAM-5.0/SKL</mark>/OpenFOAM/OpenFOAM-5.0/etc/bashrc </p><p> $ mkdir -p $FOAM_RUN </p><p> $ run </p><p> $ cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .</p><p> $ cd pitzDaily </p><p> $ blockMesh </p><p> $ simpleFoam </p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

