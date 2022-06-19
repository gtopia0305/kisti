---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Job Execution through Scheduler (PBS) > A.
  Queue Configuration
---

# A. Queue Configuration

ㅇ The commercial queue (for executing commercial SW) and the debug queue for debugging are applied with the shared node policy, where multiple jobs are assigned per node within the range of available resources (CPU core); only one job is assigned to one node in the remaining queues according to the exclusive node policy.

ㅇ Job queue

&#x20;\- The queues that can be used by general users and the number of jobs that can be submitted per user are presented in the following table. (as of April 2021)

**※ Node configuration can be adjusted while the system is in operation depending on the system loads.** (The node configuration and maximum number of jobs allowed can be checked frequently through showq command and motd)





**※ Node configuration can be adjusted while the system is in operation depending on the system loads.** (The node configuration and maximum number of jobs allowed can be checked frequently through showq command and motd)

&#x20;

① Queue description



② Limited number of job submissions

\- Maximum no. of job submissions per user : Error occurs at the time of submission if the number of allowed submissions is exceeded

\- Maximum no. of job executions per user : Previous job needs to be completed first if the number of allowed submissions is exceeded

③ Limited resource occupancy

\- No. of occupied nodes per job (max|min) : Error occurs at the time of submission if the number of occupied nodes in a single job exceeds the min. and max. range. It is not associated with the number of occupied nodes of jobs being executed or queued.

④ Distinguishing queues according to the KNL memory queue (all cluster modes are quadrant)

\- exclusive, normal, long, and debug queues are set in the cache mode (MCDRAM is used as the L3 cache), while the flat queue is set in the flat mode (MCDRAM is used as RAM along with DDR4).

\- The cache mode has a maximum available memory of 82 GB, whereas the flat mode has 102 GB for protecting the system.

⑤ By setting hyperthread as off, up to 68 threads per node can be used for KNL, and up to 40 threads per node can be used for SKL.
