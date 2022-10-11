# \[별첨11] 플랫 노드(Flat node) 사용법

\
◦ 누리온 KNL 노드 중 MCDRAM (16GB)을 DDR4와 같이 사용할 수 있는 플랫 모드 (Flat mode)를 지원하는 노드를 일반 사용자에게 서비스하고 있으며, 해당 노드를 사용하기 위해서는 Flat 큐를 지정하여 작업을 제출해야한다.

◦ Flat 모드를 사용하기 위해서는 “**numactl**”이라는 명령어를 반드시 사용해야하며, 이는 선호하는 메모리 모드 혹은 기본설정(default) 메모리 모드를 특정하는 명령어이다. 예를 들어, “my\_app.x”라는 실행파일을 flat 모드에서 실행하고자 할 경우 numactl 명령어와 함께 “-m” 옵션으로 해당되는 NUMA 노드를 아래와 같이 명시할 수 있으나, my\_app.x 실행이 MCDRAM 16G 이상의 메모리를 필요로 할 경우, 메모리 부족으로 프로그램이 종료된다. 따라서, 아래와 같이 MCDRAM을 **우선** **사용**한다는 옵션인 **“-p”**옵션을 활용하여 사용자 실행파일이 16G이상의 메모리를 필요로 한 경우에도 작업이 종료되지 않도록 사용하는 방법을 권장한다.

{% code title="■ Flat 모드 작업 스크립트 작성 예제" %}
```
#!/bin/sh
#PBS -N flat_job
#PBS -V
#PBS -q flat
#PBS -A {PBS 옵션 이름} # Application별 PBS 옵션 이름표 참고 
#PBS -l select=1:ncpus=68:mpiprocs=32:ompthreads=1
#PBS -l walltime=12:00:00

cd $PBS_O_WORKDIR

mpirun numactl -m 1 my_app.x
또는
mpirun numactl -p 1 my_app.x
```
{% endcode %}

※ pbs option으로 제출할 큐는 반드시 flat 선택 (즉, -q flat)



{% hint style="info" %}
2022년 4월 12일에 마지막으로 업데이트되었습니다.
{% endhint %}
