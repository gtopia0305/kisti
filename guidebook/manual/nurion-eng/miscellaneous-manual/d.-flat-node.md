---
description: 기술지원 > 지침서 > 사용법 > NURION(ENG) > Miscellaneous Manual > D. Flat Node
---

# D. Flat Node

◦ The node that supports the flat mode for which MCDRAM (16GB) and DDR4 can be used together among Nurion KNL nodes is provided to general users. To use this particular node, a job must be submitted by designating a flat queue.

◦ The “**numactl**” command must be used to apply the flat mode, which is a command for specifying the preferred or default memory mode. For example, when executing a file named “my\_app.x” in a flat mode, the numactl command and NUMA node corresponding to the “-m” option can be clarified together; however, if executing my\_app.x requires an MCDRAM of 16 GB or higher, the program is terminated owing to lack of memory. Therefore, it is recommended to use a method that ensures the program is not terminated when a user’s execution file requires 16 GB of memory or higher by using the **“-p”** option, which ensures that MCDRAM is **primarily** **used**.

\*\*\*\*

#### ■ **Example of a flat mode job script**

```
#!/bin/sh
#PBS -N flat_job
#PBS -V#PBS -q flat
#PBS -A {PBS option name} # refer to the table of PBS option name per application
#PBS -l select=1:ncpus=68:mpiprocs=32:ompthreads=1
#PBS -l walltime=12:00:00 

cd $PBS_O_WORKDIR 

mpirun numactl -m 1 my_app.x
or
mpirun numactl -p 1 my_app.x
```

※ Flat must be selected for the queue being submitted with the PBS option (i.e., -q flat)
