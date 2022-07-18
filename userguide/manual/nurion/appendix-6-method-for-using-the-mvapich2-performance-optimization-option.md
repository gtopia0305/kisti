# \[별첨6] MVAPICH2 성능 최적화 옵션 사용법

MPI 라이브러리로 MVAPICH2를 활용하는 경우 작업 스크립트 내에서 환경변수를 사용하여 프로세스 간 통신 알고리즘을 설정할 수 있다. 주요 키워드는 아래와 같으며, 사용자는 코드 내에서 많이 활용되는 collective communication에 대하여 환경변수 설정을 활용하여 실행 성능을 최적화할 수 있다.

<mark style="color:red;">**MV2\_INTRA**</mark> 환경변수는 노드 내 MPI 프로세스 통신에서 활용되며 <mark style="color:blue;">**MV2\_INTER**</mark> 환경변수는 노드 간 MPI 프로세스 통신에서 활용된다. MPI\_Alltoall의 경우 INTRA 및 INTER 두가지 경우에 대하여 동일한 알고리즘이 적용된다. 크기가 다른 데이터 전송을 위한 루틴(예: MPI\_Alltoallv) 또는 non-blocking 기반 루틴(예: MPI\_Ibcast)에도 동일하게 적용 가능하며 설정 방법은 아래와 같다.

■ MVAPICH2 라이브러리 collective communication 주요 환경 변수

| **MPI 통신**     | **환경변수**                                                                                                                            | **설정범위** |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------- | -------- |
| MPI\_Gather    | <p><mark style="color:red;">MV2_INTRA</mark>_GATHER_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_GATHER_TUNING</p>       | 0 \~ 3   |
| MPI\_Bcast     | <p><mark style="color:red;">MV2_INTRA</mark>_BCAST_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_BCAST_TUNING</p>         | 0 \~ 10  |
| MPI\_Scatter   | <p><mark style="color:red;">MV2_INTRA</mark>_SCATTER_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_SCATTER_TUNING</p>     | 1 \~ 5   |
| MPI\_Allreduce | <p><mark style="color:red;">MV2_INTRA</mark>_ALLREDUCE_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_ALLREDUCE_TUNING</p> | 1 \~ 6   |
| MPI\_Reduce    | <p><mark style="color:red;">MV2_INTRA</mark>_REDUCE_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_REDUCE_TUNING</p>       | 1 \~ 6   |
| MPI\_Allgather | <p><mark style="color:red;">MV2_INTRA</mark>_ALLGATHER_TUNING</p><p><mark style="color:blue;">MV2_INTER</mark>_ALLGATHER_TUNING</p> | 1 \~ 11  |
| MPI\_Alltoall  | MV2\_ALLTOALL\_TUNING                                                                                                               | 0 \~ 4   |

※ 사용 예시 (bash 스크립트 작성 경우) : export MV2\_INTER\_ALLREDUCE\_TUNING=2

※ 각 환경변수에 대한 자세한 설명은 [http://mvapich.cse.ohio-state.edu/userguide/](http://mvapich.cse.ohio-state.edu/userguide/) 참조



{% hint style="info" %}
2022년 2월 23일에 마지막으로 업데이트되었습니다.
{% endhint %}
