# 가. 개념

&#x20;  버스트버퍼 IME는 Nurion /scratch 파일시스템의 캐시 역할을 수행한다. IME를 통한 데이터 접근방법은 아래 그림과 같다.\


![](<../../../../.gitbook/assets/버스트버퍼(Burst Buffer) 사용법.png>)

&#x20;IME는 사용자 레벨 파일시스템인 FUSE(**F**ile System In **USE**rspace)를 사용하여 클라이언트 노드(전체 계산노드와 로그인노드)에 마운트 되어 있다. 이때 유의할 것은 IME는 캐시 역할을 수행하므로 /scratch 파일시스템이 사전에 반드시 마운트 되어 있어야 한다. IME 디렉터리 경로는 **/scratch\_ime** 이며 최초 사용자가 해당 디렉터리(/scratch\_ime/$USER)에 접속하면 스크래치(/scratch/$USER) 파일시스템의 모든 디렉터리와 파일의 구조를 동일하게 확인할 수 있다. 이는 실제 IME 디바이스에 존재하지 않는 데이터이며, 버스트버퍼를 이용하여 작업 수행 시 /scratch에서 IME에 캐싱하는 방식으로 사용된다. IME를 사용하기 위해서는 작업 스크립트에 버스트버퍼 프로젝트명(#PBS -P burst\_buffer)을 명시해야 한다. 어플리케이션을 수행하는 방법은 크게 아래와 같이 두가지 방식이 존재한다.

&#x20;

① IME의 마운트 포인트인 /scratch\_ime를 입출력 디렉터리로 지정하여 사용하는 방식으로 별도의 컴파일 없이 기존 POSIX 기반의 I/O를 수행할 수 있다. 즉, 사용자는 기존의 방식대로 작업을 제출하되, 자료 입출력 자료 경로만을 /scratch\_ime/$USER/ 하단에 설정하면 된다.

&#x20;    ex) INPUT="/scratch\_ime/$USER/input.dat", OUTPUT="/scratch\_ime/$USER/output.dat"

```
#!/bin/sh
#PBS -N burstbuffer
#PBS -V
#PBS -q normal        # 모든 큐 사용가능
#PBS -A {PBS 옵션 이름} # Application별 PBS 옵션 이름표 참고
#PBS -P burst_buffer  # 버스트 버퍼 사용을 위해서 반드시 명시
#PBS -l select=2:ncpus=16:mpiprocs=16
#PBS -l walltime=05:00:00

cd $PBS_O_WORKDIR

OUTFILE=/scratch_ime/$USER/output.dat  

# 이하 해당 작업 관련 실행명령어 작성 (4장 "스케줄러를 통한 작업실행" 나절 예제 참조)
```

&#x20;

② MPI-IO 기반의 I/O를 사용하기 위해서는 IME를 지원하는 mvapich2/2.3.1 모듈을 이용해야 수행이 가능하다. 응용 프로그램 또한 이 MPI 라이브러리를 이용하여  다시 컴파일 해야 한다. 또한 파일 또는 디렉터리의 경로는 아래 예시와 같이 IME 프로토콜을 사용하여 경로를 지정한다.&#x20;

&#x20;    ex) OUTFILE=ime:///scratch/$USER/output.dat (아래 작업스크립트 예제 참조)

```
$ module load mvapich2/2.3.1
```

위와 같이 mvapich2/2.3.1 모듈을 로드하고 아래와 같이 작업 스크립트를 작성한다.

&#x20;

```
#!/bin/sh
#PBS -N mvapich2_ime
#PBS -V
#PBS -q normal            # KNL에 해당하는 모든 큐 사용 가능 (전용큐, normal, long, flat, debug)
#PBS -A {PBS 옵션 이름} # Application별 PBS 옵션 이름표 참고
#PBS -P burst_buffer     # 버스트 버퍼 사용을 위해 반드시 명시
#PBS -l select=2:ncpus=16:mpiprocs=16
#PBS -l walltime=5:00:00

cd $PBS_O_WORKDIR
TOTAL_CPUS=$(wc -l $PBS_NODEFILE | awk '{print $1}')
OUTFILE=ime:///scratch/$USER/output.dat  
mpirun_rsh -np ${TOTAL_CPUS} -hostfile $PBS_NODEFILE ./a.out 
또는
mpirun -np ${TOTAL_CPUS} -hostfile $PBS_NODEFILE ./a.out 
```

&#x20;

\* 지원컴파일러: gcc/6.1.0, gcc/7.2.0, intel/17.0.5, intel/18.0.1, intel/18.0.3, intel/19.0.4, pgi/18.10

\* IME의 MPI-IO는 별도의 ROMIO 인터페이스를 사용하여 구현되었으나, MVAPICH2/2.3.1 버전부터 공식적으로 IME를 지원하는 ROMIO 기능이 포함됨. (OpenMPI는 미지원)

\* 버스트버퍼 IME는 NURION 전체 계산노드(SKL, KNL)에서 사용이 가능함.

&#x20;

&#x20;

&#x20;  IME의 데이터 관리를 위해서는 아래 그림에 있는 데이터의 라이프 사이클을 파악해야 한다. IME 데이터 처리는 크게 Prestage, Prefetch, Sync, Release 단계가 있으며 각각에 대해 IME-API(#ime-ctl) 명령을 제공하고 있다.

![](<../../../../.gitbook/assets/버스트버퍼(Burst Buffer) 사용법(1).png>)

| ime-ctl -i $INPUT\_FILE  | <p>작업 데이터를 IME로 Stage-In 실행</p><p>(/scratch에서 /scratch_ime로 데이터 캐싱)</p> |
| ------------------------ | ----------------------------------------------------------------------- |
| ime-ctl -r $OUTPUT\_FILE | <p>IME 데이터를 병렬파일시스템과 동기화<br>(/scratch_ime의 데이터를 /scratch로 전송)</p>       |
| ime-ctl -p $TMP\_FILE    | <p>IME에서 데이터 삭제</p><p>(/scratch_ime의 데이터를 삭제(purge))</p>                |
| ime-ctl -s $FILE         | IME 데이터의 상태정보 제공                                                        |

\* #ime-ctl --help를 통해 상세 옵션 확인 가능
