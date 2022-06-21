# NASTRAN 사용자 지침서

## &#x20;<a href="#jcstitle" id="jcstitle"></a>

본 문서는 누리온 시스템에서 NASTRAN 소프트웨어 사용을 위한 기초적인 정보를 제공하고 있습니다. &#x20;

따라서, NASTRAN 소프트웨어 사용법 및 누리온/리눅스 사용법 등은 포함되어 있지 않습니다. &#x20;

누리온/리눅스 사용법에 대한 정보는 KISTI 홈페이지([https://www.ksc.re.kr](https://www.ksc.re.kr/))의 자료실 내에 누리온 사용자 지침서 등을 참고하시기 바랍니다.

&#x20;\* updated: 2020. 2.&#x20;

&#x20;

## 1. 사용정책

\- 한 사용자 아이디 당 **최대 32개 CPU 코어 수행 가능**합니다.

&#x20;\- <mark style="color:red;">한정된 라이선스를 슈퍼컴 사용자들이 함께 사용하므로, 사용정책 기준을 초과하여 사용할 경우 해당 작업은 관리자가 강제 종료 합니다.</mark>

&#x20;\- 부득이하게 많은 라이선스가 필요할 경우, KISTI 홈페이지([https://www.ksc.re.kr](https://www.ksc.re.kr/))를 통해 사전에 관리자와 협의해야 합니다.&#x20;

&#x20;\- 작업 제출 전 "lic\_check" 명령 에서 메뉴 선택을 통해 라이선스 상태를 확인 한 다음 작업을 제출하시기 바랍니다.

&#x20;\- 누리온시스템 로그인 노드의 과부하 방지를 위해 pre/post 작업은 허가하지 않습니다.&#x20;

&#x20;\- 2019년 3월 PM 이후(3월14일)부터 작업제출 스크립트에 "#PBS -A nastran" 옵션을 사용해야 합니다.

&#x20;

## 2. 소프트웨어 설치 정보

### (1) 설치 버전&#x20;

&#x20;\- MSC ONE (NASTRAN) 20182

&#x20;\- MSC ONE (NASTRAN) 20191

&#x20;\- MSC ONE (NASTRAN) 20213

&#x20;

### (2) 설치 위치&#x20;

&#x20;\- 20182 : /apps/commercial/MSC/Nastran

&#x20;\- 20191 : /apps/commercial/MSC/MSC\_Nastran

&#x20;\- 20213 : /apps/commercial/MSC/MSC\_Nastran/2021.3

&#x20;

### (3) 실행 파일 경로 및 config 파일 경로&#x20;

&#x20; <20182>

&#x20;\- 실행 파일 경로: /apps/commercial/MSC/Nastran/bin

&#x20;\- Conf 파일: /apps/commercial/MSC/Nastran/conf/nast20182rc

&#x20;<20191>

&#x20;\- 실행 파일 경로 : /apps/commercial/MSC/MSC\_Nastran/2019fp1/bin

&#x20;\- Conf 파일 : /apps/commercial/MSC/MSC\_Nastran/2019fp1/conf/nast20191rc

<20213>

&#x20;\- 실행 파일 경로 : /apps/commercial/MSC/MSC\_Nastran/2021.3/bin

&#x20;\- Conf 파일 : /apps/commercial/MSC/MSC\_Nastran/2021.3/conf/nast20213rc

&#x20;

## 3. 소프트웨어 실행 방법

### (1) 실행 명령

\- 사용자의 홈디렉토리 혹은 서브 디렉토리에서 MD Nastran R2.1을 사용하는 경우 “nastran inputfile"의 형태로 입력 합니다.

&#x20;

### (2) 환경 설정

\- Nastran 실행 관련 설정을 변경하기 위해서는 환경설정 파일을 수정해 주어야 합니다.

\- 20182 버전 환경 설정 파일 경로 : /apps/commercial/MSC/Nastran/conf/nast20182rc

\- 20191 버전 환경 설정 파일 경로 : /apps/commercial/MSC/MSC\_Nastran/2019fp1/conf/nast20191rc

\- 20213 버전 환경 설정 파일 경로 : /apps/commercial/MSC/MSC\_Nastran/2021.3/conf/nast20213rc

&#x20;

\- 20161 버전 기본 환경 설정을 변경하고자 하는 경우, 위 경로의 "nast20182rc"를 홈 디렉토리나 작업 디렉토리로 복사하여 마침표를 파일이름에 추가하여 ".nast20182rc"로 파일 이름을 변경한 다음 그 파일에서 필요한 부분을 수정해서 사용하기 바랍니다.

&#x20;

\[ nast20182rc 파일 예제 ]

```
auth=27500@vaccine02.extmode=i8sdir=/scratch/applicbuffsize=65537memory=max$mpiimp=intelmpiishellpath=$MSC_BASE/msc20182/actran/linux64/Actran_18.0.b.107480/bin:$MSC_BASE/msc20182/nast:j.env=ACTRAN_PATH=$MSC_BASE/msc20182/actran/linux64j.env=ACTRAN_PRODUCTLINE=$MSC_BASE/msc20182/actran/linux64/Actran_18.0.b.107480j.env=ACTRAN_MPI=$MSC_BASE/msc20182/actran/linux64/Actran_18.0.b.107480/mpi/intelmpij.env=ACTRAN_AFFINITY=resetj.env=ACTRAN_MPI_OPTS='-pmi-connect nocache -pmi-noaggregate -genvnone -print-rank-map'$scr=yes$ End
```

&#x20;※ 사용자 홈디렉토리로 복사할 때 아래와 같이 사용하면 됩니다.

(예) cp 명령 사용 예제

```
$ cp /apps/commercial/MSC/Nastran/conf/nast20182rc ./.nast20182rc
```

&#x20;

### (3) 실행 방법

&#x20;\- Interactive 방식의 실행은 CPU time이 20분으로 제한되어 있습니다.

&#x20;\- 장시간의 계산 작업은 PBS 라는 스케줄러를 이용하여 작업을 제출해야 합니다.

&#x20;\- 작업 제출 전 "lic\_check" 명령을 실행하여 라이선스 상태를 확인 후 작업을 제출하시기 바랍니다.

&#x20;

### (4) 스케쥴러 작업 스크립트 파일 작성

&#x20;누리온 시스템에서는 로그인 노드에서 PBS 라는 스케쥴러를 사용하여 작업을 제출해야 합니다.

&#x20;누리온 시스템에서 PBS 를 사용하는 예제 파일들이 아래의 경로에 존재하므로 사용자 작업용 파일을 만들 때 이를 참고 하기 바랍니다.

&#x20; \- 예제 파일 :  /apps/commercial/test\_samples/MSC\_NASTRAN/nast\_20182.cmd ****&#x20;

&#x20;

※ 아래 예제는 누리온 시스템 에서의 NASTRAN에 대한 예제입니다.&#x20;

```
#!/bin/sh
#PBS -V
#PBS -N <span style="color:red">Nastran_job</span>
#PBS -q commercial
#PBS -l select=1:ncpus=40:mpiprocs=1:ompthreads=40
#PBS -l walltime=04:00:00
#PBS -A nastran

cd $PBS_O_WORKDIR

/apps/commercial/MSC/Nastran/bin/nast20182 car_mod_freq.bdf smp=$NCPUS batch=no sdir="."
```

```
#!/bin/sh#PBS -V#PBS -N Nastran_job#PBS -q commercial#PBS -l select=1:ncpus=40:mpiprocs=1:ompthreads=40#PBS -l walltime=04:00:00#PBS -A nastrancd $PBS_O_WORKDIR/apps/commercial/MSC/Nastran/bin/nast20182 car_mod_freq.bdf smp=$NCPUS batch=no sdir="."
```

`#!/bin/sh`\
`#PBS -V`

`code`

``

&#x20;\- 위에서 파란색으로 표기된 부분은 사용자가 적절히 수정해야 합니다.

&#x20;\- 2019년 3월 PM 이후(3월14일)부터 "#PBS -A nastran" 옵션이 없는 경우 작업제출이 되지 않습니다.

&#x20;\- **작업 제출은 스크래치 디렉토리에서만 가능** 합니다.

&#x20;\- 사용자별 스크래치 디렉토리는 /scratch/$USER입니다.

&#x20;\- 스크래치 디스크는 작업 종료 후 일정 시간(2020년 2월 현재 정책: 15일)이 지나면 삭제되기 때문에, 작업이 완료 된경우 빠른 시일 내에 백업하시길 권장합니다. &#x20;

\- 기타 PBS 에 관련된 명령어 및 사용법은 누리온 사용자 지침서를 참조하시면 됩니다.

&#x20;

### (5) 작업 제출 방법

&#x20;\- 예제 : 스크립트 파일 이름이 nastran.sh 인 경우

```
 $ qsub nastran.sh
```

&#x20;

### (6) 작업 상태 확인

```
$ qstat (또는 qstat -u $USER) 
```

&#x20;

### (7) 제출된 작업을 강제로 종료

&#x20;\- 사용 방법 : qdel {작업ID}

&#x20;\- 작업ID는 qstat 명령어 실행 시 제일 왼쪽에 표시 되는 정보입니다.(ex. 1771476.pbs)

&#x20;\- 예제 : 작업ID 가 s1771476.pbs 인 경우

```
$ qdel  1771476.pbs
```

&#x20;

### (8) PBS 상에서 사용 가능한 자원 확인

```
$ pbs_status
```

&#x20;

\
2022년 5월 11일에 마지막으로 업데이트되었습니다.
