---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Miscellaneous Manual > B. Singularity
  Container
---

# B. Singularity Container

Singularity is a container platform suitable for the HPC environment to implement the OS virtualization as a Docker. The Linux distribution version suitable for a user’s work environment, compiler, and container images containing library will be provided. The container is operated to execute a user program.

![](<../../../../.gitbook/assets/싱귤레러티 컨테이너 아키텍처.png>)

**ㅇ Loading singularity module**

```
$ module load singularity/3.6.4
```

**ㅇ Executing shell in singularity container**

```
$ singularity shell [image name] 
$ singularity shell tensorflow-1.12.0-py3.simgSingularity: Invoking an interactive shell within container... 

Singularity tensorflow-1.12.0-py3.simg:tensorflow>
```

**ㅇ Executing user program in singularity container**

```
$ singularity exec [image name] execution command 
$ singularity exec tensorflow-1.12.0-py3.simg python convolutional.py
```

_※ For executing containers through the scheduler (PBS) in a computing node, refer to the example in the guideline 'Job Submission through Scheduler (PBS)' B.-2) 'Submission of Interactive Jobs'_

_※ A convolutional model sample program (convolutional.py) and data directory (data) can be copied from the /apps/applications/tensorflow/1.12.0/examples directory to a user’s work directory to be tested._

| **Software (framework)** | **Container image file**                                          |
| ------------------------ | ----------------------------------------------------------------- |
| tensorflw 1.12.0         | /apps/applications/tensorflow/1.12.0/tensorflow-1.12.0-py3.simg   |
| tensorflw 1.13.1         | /apps/applications/singularity\_images/tensorflow-1.13.1-py3.simg |
| pytorch 1.2.0            | /apps/applications/singularity\_images/pytorch-1.2.0-py3.simg     |

**ㅇ For users to build a singularity container image without root permission**

**(Local build)**

```
 $ singularity build --fakeroot ubuntu1.sif ubuntu.def (building ubuntu1.sif image from the recipe file) 
 $ singularity build --fakeroot ubuntu2.sif library://ubuntu:18.04 (building ubuntu2.sif image from the singularity library) 
 $ singularity build --fakeroot --sandbox ubuntu3 docker://ubuntu:18.04 (building sandbox directory (ubuntu3) from Docker Hub)
```

_※ It is supported in the 3.6.4 version; go to KISTI website > Technical Support > Inquiry to request the administrator to register for the use of fakeroot._\
_※ Root permission is required to adjust the generated singularity image file (\*.sif), and it needs to be converted into a sandbox (writable chroot directory)._

**(Example of ubuntu.def recipe file)**

```
bootstrap: library
 from: ubuntu:18.04 
 %post 
 apt update 
 %runscript 
 echo "hello world from ubuntu container!"
```

**(Remote build)**

```
 $ singularity build --remote ubuntu4.sif ubuntu.def  
 (Building ubuntu4.sif image from the recipe file using a remote build service provided by Sylabs Cloud)
```

_※ An access token needs to be generated and registered on Nurion to use a remote build service provided by Sylabs Cloud (https://cloud.sylabs.io) \[reference 1]_\
_※ Generating/managing a singularity container image is possible by accessing Sylabs Cloud on a web browser \[reference 2]_

**(Import/export singularity container image)**

```
$ singularity pull tensorflow.sif library://dxtr/default/hpc-tensorflow:0.1 (importing a container image from the Sylabs Cloud library)
$ singularity pull tensorflow.sif docker://tensorflow/tensorflow:latest (importing an image from Docker Hub and converting it into a singularity image)
$ singularity push -U tensorflow.sif library://ID/default/tensorflow.sif (exporting a singularity image to the Sylabs Cloud library (upload))
```

_※ An access token needs to be generated and registered on Nurion to export an image to Sylabs Cloud (https://cloud.sylabs.io) \[reference 1]_

\\

**\[Reference 1] Generating a Sylabs Cloud access token and registering on Nurion**

![](<../../../../.gitbook/assets/Sylabs Cloud 계정 등록 및 로그인 하기.png>)

![](<../../../../.gitbook/assets/새로운 토큰 생성하기.png>)

![](<../../../../.gitbook/assets/클립보드로 토큰 복사하기.png>)

![](<../../../../.gitbook/assets/토큰 입력하기.png>)

**\[Reference 2] Building a singularity container image using a remote builder on a web browser**

![](<../../../../.gitbook/assets/웹 브라우저에서 컨테이너 이미지 빌드하기.png>)

![](<../../../../.gitbook/assets/빌드한 컨테이너 이미지 목록 보기.png>)

_※ Includes a list of images remotely built with singularity commands in Nurion_
