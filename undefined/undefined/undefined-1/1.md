---
description: '[별첨1] 작업 스크립트 주요 키워드'
---

# \[별첨1] 작업 스크립트 주요 키워드

\
&#x20;작업 스크립트 내에서 적절한 키워드를 사용하여 원하는 작업을 위한 자원 할당 방법을 명시해야 한다. 주요 키워드는 아래와 같으며, 사용자는 이들 중에서 몇 가지만 사용하여 작업 스크립트 파일을 작성할 수 있다.&#x20;

&#x20;

**- job-name ( -J, --job-name )**

&#x20;작업의 이름을 지정하며, 명시하지 않으면 스크립트 파일 이름이 작업 이름으로 지정된다.

&#x20;

**- time ( -t, --time )**

&#x20;예상되는 작업 소요 시간을 의미하며, 실제 예상되는 작업 소요 시간보다 약간 더 길게 설정해 주는 것이 안전하다. 해당 파티션의 Wall time limit을 초과하면, 작업이 제출되지 않는다. 지정된 시간에 이르렀는데 작업이 완료되지 않으면 SLURM 스케줄러가 작업을 강제 종료시킨다.

&#x20;

**- partition ( -p, --partition )**

&#x20;작업 수행을 위한 SLURM 파티션을 지정한다. 파티션명은 sinfo 명령어로 확인이 가능하다.

&#x20;

**- nodes ( -N, --nodes )**

&#x20; 작업을 위해 할당할 노드의 수를 지정한다.

&#x20;

**- ntasks ( -n, --ntasks )**

&#x20; 작업을 위해 할당할 프로세스의 수를 지정한다.

&#x20;

**- tasks-per-node ( --tasks-per-node )**

&#x20; 노드당 할당할 프로세스의 수를 지정한다.

&#x20;

**- input ( -i, --input )**

&#x20; Standard input을 지정한다.

&#x20;

**- cpus-per-task ( -c, --cpus-per-task )**

&#x20;  작업 태스크 당 필요한 CPU 개수를 명시한다.

**- output ( -o, --output )**

&#x20; Standard output을 지정한다.

&#x20;   %x : "job name" 으로 지정한 명칭을 파일 명으로 사용한다.

&#x20;   %j : 작업 제출 시 부여되는 "job ID" 를 파일 명으로 사용한다.

&#x20;   %a : "job array ID" (index) 번호를 파일 명으로 사용한다.

&#x20;   %u : "user ID" 를 파일 명 으로 사용한다.

&#x20;

**- error ( -e, --error )**

&#x20;  Standard error를 지정한다.

&#x20;   %x : "job name" 으로 지정한 명칭을 파일 명으로 사용한다.

&#x20;   %j : 작업 제출 시 부여되는 "job ID" 를 파일 명으로 사용한다.

&#x20;   %a : "job array ID" (index) 번호를 파일 명으로 사용한다.

&#x20;   %u : "user ID"를 파일 명으로 사용한다.

&#x20;

**- dependency ( -d, --dependency )**

&#x20; ****  작업 의존성을 설정한다. 설정된 작업이 종료된 후에 작업이 시작된다. &#x20;

&#x20;

&#x20;

※ 상세 매뉴얼 : [http://slurm.schedmd.com/](http://slurm.schedmd.com/) 참조
