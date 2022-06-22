# 누리온 LS-DYNA 사용자 지침서(2020.02)

본 문서는 누리온 시스템에서 LS-DYNA 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다. &#x20;

따라서, LS-DYNA 소프트웨어 사용법 및 누리온/리눅스 사용법 등은 포함되어 있지 않습니다. &#x20;

누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지 (https://www.ksc.re.kr)의 기술지원 > 지침서 내 누리온 사용자 지침서 등을 참고하시기 바랍니다. &#x20;

&#x20;

### **1. 사용 정책**&#x20;

&#x20;\- 사용자별 **최대 40개 CPU 코어 수행 가능**합니다.

&#x20;\- 한정된 라이선스를 슈퍼컴 사용자들이 함께 사용하므로, 사용정책 기준을 초과하여 사용할 경우 해당 작업은 관리자가 강제 종료 합니다.

&#x20;\- 부득이하게 많은 라이선스가 필요할 경우, KISTI 홈페이지(https://www.ksc.re.kr)를 통해 사전에 관리자와 협의해야 합니다.&#x20;

&#x20;\- 작업 제출 전 "lic\_check" 명령 에서 메뉴 선택을 통해 라이선스 상태를 확인 한 다음 작업을 제출하시기 바랍니다.

&#x20;\- 누리온시스템 로그인 노드의 과부하 방지를 위해 pre/post 작업은 허가하지 않습니다.&#x20;

&#x20;\- 2019년 3월 PM 이후

(3월14일)

에는 작업제출 스크립트에 "

\#PBS -A lsdyna" 옵션을 사용해야 합니다.

&#x20;

### **2. 소프트웨어 설치 정보**

**(1) 설치 버전**&#x20;

&#x20;\- LSDYNA R11.1.0(smp, mpp), R10.1.0(smp, mpp), R9.2.0(smp, mpp), R9.1.0(smp, mpp)

&#x20;

**(2) 설치 위치**&#x20;

&#x20;\-  /apps/commercial/LSDYNA

&#x20;

### **3. 소프트웨어 실행 방법**

**(1) 실행 방법 설명 및 실행 예시**

\- 실행 명령 및 옵션

&#x20;: 옵션 대소문자 및 위치 구분 없음. 해당 옵션 사용하지 않을 경우 default값으로 출력됨

&#x20;

&#x20;(Options = Input file specification, Output file specification, Extra option)

&#x20;

| <p> [<strong>ls-dyna 명령어</strong>] [<strong>option</strong>]<br></p> |
| -------------------------------------------------------------------- |

※ **ls-dyna 명령어**는 **(2)실행 예제** 및 관련 표를 참고하시고,  **option**에 대한 정보는 아래 표를 참고하여 사용하시길 바랍니다.

&#x20;

|  **구분**                                                                   |  **option**                                                                |  **설명**                                        |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------- |
| <p> <strong>Input file</strong></p><p><strong>specification</strong></p>  |  i=inf                                                                     |  input file                                    |
|  m=sif                                                                    |  stress initialization file                                                |                                                |
|  I=isf2                                                                   |  interface segment save file to be used                                    |                                                |
|  t=tpf                                                                    |  optional temperature file                                                 |                                                |
| <p> <strong>Output file</strong></p><p><strong>specification</strong></p> |  o=otf                                                                     |  high speed printer file (default=d3hsp)       |
|  g=ptf                                                                    |  binary plot file for graphics (default=d3plot)                            |                                                |
|  d=dpf                                                                    |  dump file for restarting (default=d3dump)                                 |                                                |
|  f=thf                                                                    |  binary plot file for time histories (default=d3thdt)                      |                                                |
|  u=xtf                                                                    |  binary plot file for time extra data (default=xtfile)                     |                                                |
|  a=rrd                                                                    |  restart dump file (default=runrsf)                                        |                                                |
|  s=iff                                                                    |  interface force file                                                      |                                                |
|  z=isf1                                                                   |  interface segment save file to be created                                 |                                                |
|  b=rlf                                                                    |  binary plot file for dynamic relaxation (default=d3rfl)                   |                                                |
|  bem=bof                                                                  |  calculated acoustic output file                                           |                                                |
|  d3prop=d3prop                                                            |  output property data                                                      |                                                |
|  **Extra Option**                                                         |  x=scl                                                                     |  scale factor for binary file size (default=7) |
|  c=cpu                                                                    |  cpu time limit (in seconds)                                               |                                                |
|  memory=Nwds                                                              |  number of words to be allocated                                           |                                                |
|  memory2=Nwds2                                                            |  number of words to be allocated in each processor (extra option for mpp)  |                                                |
|  ncpu=Ncpu                                                                |  maximum number of cpus to be used                                         |                                                |
|  parn=npara                                                               |  flag for parallel force assembly                                          |                                                |
|  endtime=time                                                             |  termination time                                                          |                                                |
|  ncycle=ncycle                                                            |  termination cycle                                                         |                                                |
|  jobid=job\_name                                                          |  character string which acts as a prefix for all output files              |                                                |

&#x20;

**(2) 실행 예제**

&#x20;**1) SMP 버전 실행 예시**

&#x20;\- input file 명 : airbag\_deploy.k

&#x20;\- 사용 CPU 수 : 4개

&#x20;\- 사용메모리 양 : 400MB (100MB X 4)

&#x20;\- Output files

&#x20; : information outputs :d3hsp, messag, status.out&#x20;

&#x20; : binary outputs : d3plot - d3plot##, d3dump01 - d3dump##,&#x20;

&#x20; : ascii outputs : abstat, glstat, matsum, rcforc, rwforc, etc.

&#x20;

&#x20;**- 실행 예제**

|   $ **\[LS-DYNA 명령어]** i=airbag.deploy.k memory=100m ncpu=4 |
| ----------------------------------------------------------- |

&#x20;※ 사용가능한 LS-DYNA 명령어는 아래 표를 참고하여 선택한다.

&#x20;

|  **버전**            |   **type**                                                                                     |  **LS-DYNA 명령어 위치 및 파일명**                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
|  **R11.1.0**       |  Single Precision                                                                              |  /apps/commercial/LSDYNA/SMP/R11.1.0/ls-dyna\_smp\_s\_R11\_1\_0\_x64\_redhat65\_ifort160\_sse2 |
|  Double Precision  |  /apps/commercial/LSDYNA/SMP/R11.1.0/ls-dyna\_smp\_d\_R11\_1\_0\_x64\_redhat65\_ifort160\_sse2 |                                                                                                |
|  **R10.1.0**       |  Single Precision                                                                              |  /apps/commercial/LSDYNA/SMP/R10.1.0/ls-dyna\_smp\_s\_r1010\_x64\_redhat5\_ifort160            |
|  Double Precision  |  /apps/commercial/LSDYNA/SMP/R10.1.0/ls-dyna\_smp\_d\_r1010\_x64\_redhat5\_ifort160            |                                                                                                |
|  **R9.2.0**        |   Single Precision                                                                             |  /apps/commercial/LSDYNA/SMP/R9.2/ls-dyna\_smp\_s\_r920\_x64\_redhat59\_ifort160               |
|   Double Precision |  /apps/commercial/LSDYNA/SMP/R9.2/ls-dyna\_smp\_d\_r920\_x64\_redhat59\_ifort160               |                                                                                                |
|  **R9.1.0**        |  Single Precision                                                                              |  /apps/commercial/LSDYNA/SMP/R9.1.0/ls-dyna\_smp\_s\_r910\_x64\_redhat56\_ifort131             |
|  Double Precision  |  /apps/commercial/LSDYNA/SMP/R9.1.0/ls-dyna\_smp\_d\_r910\_x64\_redhat56\_ifort131             |                                                                                                |

&#x20;

**2) MPP 버전 실행 예시**

&#x20;\- input file 명 : airbag\_deploy.k

&#x20;\- 사용 CPU 수 : 4개

&#x20;\- 사용메모리 양 : 400MB (100MB X 4)

&#x20;\- Output files

&#x20; : Information outputs : d3hsp, mes0000 - mes0003, status.out, adptmp, scr0000 - scr0003

&#x20; : Binary outputs : d3plot - d3plot##, d3dump01.0000 - d3dump##.0003, d3full01 - d3full##

&#x20; : Binary database (smp의 ascii output에 해당) : binout0000

&#x20;

&#x20; : Ascii ouput extractions from binary database : l2a binout\* 실행&#x20;

&#x20;

&#x20;**- 실행 예제**

|   $ mpirun -np 4 **\[LS-DYNA 명령어]** i=airbag.deploy.k memory=100m memory2=20m -procs 4 |
| -------------------------------------------------------------------------------------- |

&#x20;※ 사용가능한 LS-DYNA 명령어는 아래 표를 참고하여 선택한다.

&#x20;&#x20;

|  **버전**            |   **type**                                                                                                       |  **LS-DYNA 명령어 위치 및 파일명**                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
|  **R11.1.0**       |  Single Precision                                                                                                |  /apps/commercial/LSDYNA/MPP/R11.1.0/ls-dyna\_mpp\_s\_R11\_1\_0\_x64\_centos54\_ifort160\_sse2\_platformmpi      |
|  Double Precision  |  /apps/commercial/LSDYNA/MPP/R11.1.0/ls-dyna\_mpp\_d\_R11\_1\_0\_x64\_centos54\_ifort160\_sse2\_platformmpi      |                                                                                                                  |
|  **R10.1.0**       |  Single Precision                                                                                                |  /apps/commercial/LSDYNA/MPP/R10.1.0/ls-dyna\_mpp\_s\_r10\_1\_123355\_x64\_redhat54\_ifort160\_sse2\_platformmpi |
|  Double Precision  |  /apps/commercial/LSDYNA/MPP/R10.1.0/ls-dyna\_mpp\_d\_r10\_1\_123355\_x64\_redhat54\_ifort160\_sse2\_platformmpi |                                                                                                                  |
|  **R9.2.0**        |  Single Precision                                                                                                |  /apps/commercial/LSDYNA/MPP/R9.2.0/ls-dyna\_mpp\_s\_r9\_2\_119543\_x64\_redhat54\_ifort131\_sse2\_platformmpi   |
|  Double Precision  |  /apps/commercial/LSDYNA/MPP/R9.2.0/ls-dyna\_mpp\_d\_r9\_2\_119543\_x64\_redhat54\_ifort131\_sse2\_platformmpi   |                                                                                                                  |
|  **R9.1.0**        |  Single Precision                                                                                                |  /apps/commercial/LSDYNA/MPP/R9.1.0/ls-dyna\_mpp\_s\_r9\_1\_113698\_x64\_redhat54\_ifort131\_sse2\_platformmpi   |
|   Double Precision |  /apps/commercial/LSDYNA/MPP/R9.1.0/ls-dyna\_mpp\_d\_r9\_1\_113698\_x64\_redhat54\_ifort131\_sse2\_platformmpi   |                                                                                                                  |

&#x20;

**(3) 실행 방법**

&#x20;\- Interactive 방식의 실행은 CPU time이 10분으로 제한되어 있습니다.

&#x20;\- 장시간의 계산 작업은 PBS 라는 스케줄러를 이용하여 작업을 제출해야 합니다.

&#x20;\- 작업 제출 전 "lic\_check" 명령을 실행하여 라이선스 상태를 확인 후 작업을 제출하시기 바랍니다.

&#x20;****&#x20;

**(4) 스케쥴러 작업 스크립트 파일 작성**

&#x20;누리온 시스템에서는 로그인 노드에서 PBS 라는 스케쥴러를 사용하여 작업을 제출해야 합니다.&#x20;

&#x20;

&#x20;누리온 시스템에서 PBS 를 사용하는 예제 파일들이 아래의 경로에 존재하므로 사용자 작업용 파일을 만들 때 이를 참고하시기 바랍니다.&#x20;

&#x20; \- 예제 파일 :  /apps/commercial/test\_samples/LSDYNA/lsdyna\_smp.sh **(단일 노드에서 수행)** &#x20;

&#x20;

&#x20;

&#x20;                   /apps/commercial/test\_samples/LSDYNA/lsdyna\_mpp.sh **(멀티 노드에서 수행)**&#x20;

&#x20;

※ 아래 예제는 누리온 시스템 에서의 LS-DYNA에 대한 예제입니다. **(단일 노드에서 수행)**

| <p>#!/bin/sh</p><p>#PBS -V</p><p>#PBS -N lsdyna_job</p><p>#PBS -q commercial</p><p>#PBS -l select=1:ncpus=40:mpiprocs=1:ompthreads=40</p><p>#PBS -l walltime=04:00:00</p><p>#PBS -A lsdyna</p><p> </p><p>cd $PBS_O_WORKDIR</p><p> </p><p>module load lsdyna/smp</p><p> </p><p> </p><p>/apps/commercial/LSDYNA/SMP/R10.1.0/ls-dyna_smp_s_r1010_x64_redhat5_ifort160 \</p><p>i=airbag.deploy.k memory=1000m ncpu=$NCPUS</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

\- 2019년 3월 PM 이후

(3월14일)

&#x20;부터는 "#PBS -A lsdyna" 옵션이 없는 경우 작업제출이 되지 않습니다.

&#x20;

※ 아래 예제는 누리온 시스템 에서의 LS-DYNA에 대한 예제입니다. **(멀티 노드에서 수행)**

&#x20;

&#x20;

| <p>#!/bin/sh</p><p>#PBS -V</p><p>#PBS -N lsdyna_job</p><p>#PBS -q commercial</p><p>#PBS -l select=2:ncpus=20:mpiprocs=20:ompthreads=1</p><p>#PBS -l walltime=04:00:00</p><p>#PBS -A lsdyna</p><p> </p><p>cd $PBS_O_WORKDIR</p><p> </p><p>module load lsdyna/mpp</p><p> </p><p>mpirun -machinefile $PBS_NODEFILE \</p><p>/apps/commercial/LSDYNA/MPP/R10.1.0/ls-dyna_mpp_s_r10_1_123355_x64_redhat54_ifort160_sse2_platformmpi \</p><p>i=airbag.deploy.k memory=1000m</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

&#x20;\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

&#x20;\- 2019년 3월 PM 이후

(3월14일)

&#x20;부터는 "#PBS -A lsdyna" 옵션이 없는 경우 작업제출이 되지 않습니다.

&#x20;\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

&#x20;\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.

&#x20;\- 스크래치 디스크는 작업 종료 후 일정 시간(2020년 2월 현재 정책: 15일)이 지나면 삭제되기 때문에, 작업이 완료 된경우 빠른 시일 내에 백업하시길 권장합니다.&#x20;

&#x20;

&#x20;\- 기타 PBS에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하시면 됩니다.

&#x20;

**(5) 작업 제출 방법**

&#x20;\- 예제 : 스크립트 파일 이름이 lsdyna.sh 인 경우

|  $ qsub lsdyna.sh |
| ----------------- |

&#x20;

**(6) 작업 상태 확인**

|  $ qstat (또는 qstat -u $USER)  |
| ----------------------------- |

&#x20;****&#x20;

**(7) 제출된 작업을 강제로 종료**

&#x20;\- 사용 방법 : qdel&#x20;

&#x20;\- 작업ID는 qstat 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 1771476.pbs)

&#x20;\- 예제 : 작업ID 가 1771476.pbs 인 경우

|  $ qdel 1771476.pbs |
| ------------------- |

&#x20;

**(8) PBS 상에서 사용 가능한 자원 확인**

|  $ pbs\_status |
| -------------- |

&#x20;
