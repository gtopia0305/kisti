# 뉴론 AMD 노드 활용

&#x20;

다음은 KISTI 뉴론 시스템의 AMD 노드와 Cascade 노드의 CPU 만을 활용한 application 간 성능을 보여주는 예제이다.

&#x20;

### 1. 개요

| 비교 노드        | cas\_v100\_2                      | amd               |
| ------------ | --------------------------------- | ----------------- |
| core 수       | 1, 10, 20, 40                     | 1, 10, 20, 40, 64 |
| 컴파일러         | GNU, intel                        |                   |
| MPI          | OpenMPI, impi                     |                   |
| 라이브러리        | fftw3\_single, fftw3\_double, MKL |                   |
| applications | NAMD, GROMACS, OpenFOAM           |                   |

&#x20;

### 2. applications 별 성능 분석

**1) NAMD 단일 코어**

![](https://blog.kakaocdn.net/dn/b4UI15/btq4hp32LRv/KhE7iPjTVhofXC6n76X3b0/img.png)

* 단순 컴파일러 기준 gcc 는 AMD 노드에서 보다 좋은 성능을 보여줌
* 단순 컴파일러 기준 intel 은 Cascade 노드에서 보다 좋은 성능을 보여줌

&#x20;

**2) NAMD 멀티 코어**

![](https://blog.kakaocdn.net/dn/c3r9o8/btq4kbX5BtM/CjkxZ5ByrvqyUiqdnJ34Wk/img.png)

* gcc 의 경우 AMD 노드에서 지속적으로 좋은 성능을 보여줌
* intel 의 경우 10 core 까지는 Cascade 노드에서 더 좋은 성능을 보이지만 20 core 부터는 AMD 노드에서 더 좋은 성능을 보여줌

&#x20;

**3) GROMACS 단일 코어**

![](https://blog.kakaocdn.net/dn/xGVgR/btq4eLUcNyf/wIWLus8EUI66AL6GsZke6K/img.png)

* gcc 와 intel 컴파일러 모두 Cascade 노드에서 더 좋은 성능을 보여줌

&#x20;

**4) GROMACS 멀티 코어**

![](https://blog.kakaocdn.net/dn/ch7nXV/btq4kETyt4f/HDapdj2GA5WvBtpc2jng00/img.png)

* 10 core 에서 intel 은 여전히 Cascade 노드에서 더 좋은 성능을 보여줌
* 10 core 에서 gcc는 AMD 노드에서 더 좋은 성능을 보이기 시작함
* 20 core 이후부터 gcc, intel 모두 AMD 노드에서 더 좋은 성능을 보여줌

&#x20;

**5) OpenFOAM 단일 코어**

![](https://blog.kakaocdn.net/dn/bHaXN5/btq4hpKgVNS/wooTLQ8VMAp3JOxiRQYU90/img.png)

* gcc 는 AMD 노드에서 더 좋은 성능
* intel 은 Cascade 노드에서 더 좋은 성능

&#x20;

**6) OpenFOAM 멀티 코어**

![](https://blog.kakaocdn.net/dn/nRa5d/btq4jP2AoOL/k86ZqqDl1WLI6fW8WWL2E0/img.png)

* gcc 의 경우 AMD 노드에서 지속적으로 좋은 성능을 보여줌
* intel 의 경우 10 core 까지는 Cascade 노드에서 더 좋은 성능을 보이지만 20 core 부터는 AMD 노드에서 더 좋은 성능을 보여줌&#x20;

&#x20;
