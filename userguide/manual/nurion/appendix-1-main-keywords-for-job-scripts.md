---
description: '[별첨1] 작업 스크립트 주요 키워드'
---

# \[별첨1] 작업 스크립트 주요 키워드

\
작업 스크립트 내에서 적절한 키워드를 사용하여 원하는 작업을 위한 자원 할당 방법을 명시해야 한다. 주요 키워드는 아래와 같으며, 사용자는 이들 중에서 몇 가지만 사용하여 작업 스크립트 파일을 작성할 수 있다.

| 옵션              | 형식                                          | 설명                              |
| --------------- | ------------------------------------------- | ------------------------------- |
| -A              |                                             | 어카운팅 문자열 지정                     |
| -a              | \[\[\[\[YYYY]MM]DD]hhmm\[.S]]               | 예약된 실행                          |
| -C              | ""                                          | PBS 지시어의 접두어(ex>#PBS) 변경        |
| -c              | \[c \| c=numeric \| w \| w= \| n \| s \| u] | Job의 체크포인트 주기 지정                |
| -e              |                                             | error 파일의 경로 지정                 |
| -h              |                                             | Job 실행 지연(지연실행)                 |
| -I \| -X        |                                             | X11 포워딩 활성화된 Interactive Job    |
| -J              | ]                                           | Job 배열 정의                       |
| -j              | \[o \| e]                                   | output과 error 파일 병합             |
| -k              | \[o \| e]                                   | 실행 호스트에서 output과 error 파일 유지 여부 |
| -l              |                                             | Job 리소스 요청                      |
| -M              |                                             | 이메일 받는 사람 리스트 설정                |
| -m              |                                             | 이메일 알람 지정                       |
| -N              |                                             | Job 이름 지정                       |
| -o              |                                             | output 파일의 경로 지정                |
| -P              |                                             | 프로젝트 이름 지정                      |
| -p              |                                             | Job 우선순위 설정                     |
| -q              |                                             | 서버나 큐의 이름 지정                    |
| -r              | \[Y \| N]                                   | 재실행 가능여부 마킹                     |
| -S              |                                             | 사용할 스크립트 언어 지정                  |
| -u              |                                             | Job의 사용자 ID 지정                  |
| -V              |                                             | 환경변수 내보내기                       |
| -v              |                                             | 환경변수들 확장                        |
| -W              |                                             | Job의 속성 값 설정                    |
| -W block=       |                                             | Job의 완료를 기다리는 qsub 요청           |
| -W depend=      |                                             | Job의 의존성 지정                     |
| -W group\_list= |                                             | Job 소유자의 group ID 지정            |
| -W pwd=         | ""                                          | 작업 당 password 방식                |
| -W run\_count=  |                                             | 작업이 실행되어지는 횟수 제어                |
| -W sandbox=     | \[HOME \| PRIVATE]                          | 스테이징 디렉터리와 실행 디렉터리              |
| -W stagein=     |                                             | input 파일 스테이징                   |
| -W stageout=    |                                             | output 파일 스테이징                  |
| -W unmask=      |                                             | UNIX의 JOB umask 지정              |
| -X              |                                             | Interactive Job으로 부터의 X output  |
| -z              |                                             | 함축된 작업 식별자                      |

※ 상세 매뉴얼 : [https://pbsworks.com/pdfs/PBSUserGuide14.2.pdf](https://pbsworks.com/pdfs/PBSUserGuide14.2.pdf) 참조

\\
