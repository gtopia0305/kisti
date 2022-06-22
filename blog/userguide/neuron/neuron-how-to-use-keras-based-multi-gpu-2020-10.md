# 뉴론 Keras 기반 Multi GPU 사용 방법 (2020.10)

&#x20; Keras(케라스)는 파이썬으로 작성된 오픈 소스 신경망 라이브러리로, MXNet, Deeplearning4j, 텐서플로, Microsoft Cognitive Toolkit 또는 Theano 위에서 수행할 수 있는 High-level Neural Network API이다. 케라스의 특징은 User friendliness, Modularity, Easy Extensibility로 Multi-GPU를 사용하고자 하는 사용자도 코드를 최소한으로 수정하여 쉽게 Multi-GPU를 사용할 수 있도록 하고 있다.

&#x20;

KISTI GPU 클러스터인 NEURON의 큐 구성은 다음과 같으며, ivy\_k40\_2, ivy\_v100\_2, cas\_v100\_2, cas32c\_v100\_2, cas\_v100nv\_4 큐에는 한 노드에 2, 4개의 GPU가 장착되어 있어, 단일노드를 이용할 때에도 GPU를 여러 대 사용하여 신경망 학습을 할 수 있는 환경이 구축되어 있다.&#x20;

&#x20;

&#x20;

&#x20;

\<NEURON 큐 구성 (2020.10월 기준)>

|  큐명                                | <p>할당</p><p>노드 수 </p>             | <p>Total CPU</p><p>core 수 </p>    | 작업제출개수제한 \*                         | 리소스점유제한 \*\*  |  비고 |                                 |
| ---------------------------------- | --------------------------------- | --------------------------------- | ----------------------------------- | ------------- | --- | ------------------------------- |
| <p> 사용자별</p><p>최대제출</p><p>작업개수</p> | <p>사용자별</p><p>최대실행</p><p>작업개수</p> | <p>작업별</p><p>최대노드</p><p>점유개수 </p> | <p>사용자별</p><p>최대GPU</p><p>점유개수 </p> |               |     |                                 |
| ivy\_k40\_2                        | 4                                 | 80                                | 2                                   | -             | 6   | K40 2ea탑재                       |
| ivy\_v100\_2                       | 21                                | 420                               | 4                                   | -             | 16  | V100 2ea탑재                      |
| cas\_v100\_2                       | 15                                | 600                               | 4                                   | -             | 20  | V100 2ea탑재                      |
| cas\_v100nv\_4                     | 4                                 | 160                               | 2                                   | -             | 12  | <p>V100(NVlink)</p><p>4ea탑재</p> |
| cas32c\_v100\_2                    | 5                                 | 160                               | 2                                   | -             | 6   | V100 2ea탑재                      |
| skl                                | 10                                | 360                               | 2                                   | -             | -   |                                 |
| bigmem                             | 2                                 | 96                                | 1                                   | -             | -   |                                 |
| amd                                | 2                                 | 128                               | 1                                   | -             | -   |                                 |
|  opatne                            | 1                                 | 24                                | 1                                   | -             | -   |                                 |

**※ 노드 구성은 시스템 부하에 따라 시스템 운영 중에 조정될 수 있음.**

**※ cas32c\_v100\_2 파티션 계산노드는 Xeon Gold 6242 CPU 2ea 탑재 (총 32코어)**

&#x20;

&#x20;

&#x20; 본 Multi-GPU 사용 방법 설명서에서는 CNN기반 신경망을 cifar10 데이터를 이용하여 학습하는 예제를 이용하였다.

**1.학습에 사용된 신경망 코드 (cifar10.py)**

![](https://t1.daumcdn.net/cfile/tistory/997C723F5D818DBB06)![](https://t1.daumcdn.net/cfile/tistory/99E2DA3D5D818DE51F)![](https://t1.daumcdn.net/cfile/tistory/996A393D5D818DE61D)![](https://t1.daumcdn.net/cfile/tistory/99A4D23D5D818DE620)![](https://t1.daumcdn.net/cfile/tistory/993150485D81944509)

&#x20;

&#x20;

&#x20;

![](https://t1.daumcdn.net/cfile/tistory/992B3D475D8192ED01)

&#x20;

![](https://t1.daumcdn.net/cfile/tistory/9943AB4F5D8194A005)

&#x20;

&#x20;

&#x20;

&#x20;

**2. 단일 GPU 사용 작업 제출 방법**

&#x20;

※작업제출 스크립트

| <p>#!/bin/sh</p><p>#SBATCH -J keras</p><p>#SBATCH --time=24:00:00</p><p>#SBATCH -o %x_%j.out</p><p>#SBATCH -e %x_%j.err</p><p>#SBATCH -p cas_v100_2</p><p>#SBATCH --comment tensorflow</p><p>#SBATCH --gres=gpu:2</p><p>#SBATCH -N 1</p><p> </p><p>module load python/3.7.1</p><p>source activate tf_gpu</p><p> </p><p>srun python cifar10.py</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

&#x20;

\- Conda를 이용하여 Keras 패키지가 설치된 tf\_gpu 가상환경을 구축하고 tf\_gpu 환경에서 수행하는 방법임. (Conda의 활용은 ‘KISTI 홈페이지 > 기술지원 > 지침서 > 소프트웨어 > Conda의 활용’ 참고.)

\- Keras는 tensorflow 위에서 동작하기 때문에 application명으로 tensorflow 사용.

\- ivy\_v100\_2노드에는 GPU가 2개 장착되어 있기 때문에 2개 GPU를 모두 사용할 수 있지만, 코드에 Multi-GPU를 사용한다고 명시하지 않았기 때문에 —gres옵션으로 gpu를 2개 사용한다고 하여도 하나의 gpu만을 사용한다는 것을 확인할 수 있음.

&#x20;

\<GPU 모니터링 – 단일 GPU 사용 확인>

![](https://t1.daumcdn.net/cfile/tistory/999A06445D8185B82E)

&#x20;

&#x20;

**3. Multi-GPU 사용을 위한 코드 변경 및 작업 제출 방법**

&#x20;

1\) \[from keras.utils import multi\_gpu\_model] 모듈 추가

| <p>from __future__ import print_function</p><p>import keras</p><p>from keras.datasets import cifar10</p><p>from keras.preprocessing.image import ImageDataGenerator</p><p>from keras.models import Sequential</p><p>from keras.layers import Dense, Dropout, Activation, Flatten</p><p>from keras.layers import Conv2D, MaxPooling2D</p><p>from keras.utils import multi_gpu_model</p><p>import os</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

&#x20;

2\) 코드 내 multi-gpu사용 선언

| <p># initiate RMSprop optimizer</p><p>opt = keras.optimizers.rmsprop(lr=0.0001, decay=1e-6)</p><p> </p><p>#multi-gpu</p><p>model = multi_gpu_model(model, gpus=2)</p><p> </p><p># Let's train the model using RMSprop</p><p>model.compile(loss='categorical_crossentropy',</p><p>optimizer=opt,</p><p>metrics=['accuracy'])</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\-사용하고자하는 GPU 개수만큼 gpus를 설정.\
(ex. skl\_v100nv\_4노드의 경우에는 gpus=4라고 설정)

&#x20;

3\) Multi-GPU 사용 작업 제출 방법

※작업제출 스크립트 (단일 GPU 사용과 동일)

| <p>#!/bin/sh</p><p>#SBATCH -J keras</p><p>#SBATCH --time=24:00:00</p><p>#SBATCH -o %x_%j.out</p><p>#SBATCH -e %x_%j.err</p><p>#SBATCH -p cas_v100_2</p><p>#SBATCH --comment tensorflow</p><p>#SBATCH --gres=gpu:2</p><p>#SBATCH -N 1</p><p> </p><p>module load python/3.7.1</p><p>source activate tf_gpu</p><p> </p><p>srun python cifar10.py</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

&#x20;

&#x20;

\<GPU 모니터링 – Multi-GPU 사용 확인>

![](https://t1.daumcdn.net/cfile/tistory/99B49C455D8185CE2E)

&#x20;
