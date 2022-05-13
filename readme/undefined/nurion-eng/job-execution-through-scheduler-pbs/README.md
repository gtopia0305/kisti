---
description: Job Execution through Scheduler (PBS)
---

# Job Execution through Scheduler (PBS)

The job scheduler of the 5th supercomputer Nurion uses a portable batch system (PBS). In this chapter, a method for submitting jobs through a scheduler and the related commands are introduced. The queues for users to submit jobs are already determined. The maximum number of jobs that a user can submit per queue is limited, which can be adjusted depending on the load of the system.

&#x20;

Nurion employs an **exclusive node allocation policy** as the default to ensure that only the jobs of one user are executed in one node. It prevents significant performance degradation of the user application, which can occur when the shared node allocation policy is applied. However, queues that cannot use commercial SW are applied with the shared node policy to ensure efficient use of resources because the node scale is relatively small.

&#x20;

A user’s job can only be submitted through the login node, and general users cannot directly access the computing node.

&#x20;

Moreover, **user jobs can only be submitted in /scratch**.



**A.** [**Queue Configuration**](a.-queue-configuration.md)****

**B.** [**Job Submission and Monitoring**](b.-job-submission-and-monitoring.md)****

**C.** [**Job Control**](c.-job-control.md)****
