---
description: 기술지원 > 지침서 > 사용법 > NURION(ENG) > Miscellaneous Manual > C. Burst Buffer
---

# C. Burst Buffer

**◦ Concept**

&#x20;Burst buffer IME performs the role of a cache in the Nurion /scratch filesystem. The data access method through IME is as shown in the figure below.

![](<../../../../.gitbook/assets/버스트버퍼(Burst Buffer) 사용법.png>)

&#x20;IME is mounted on a client node (entire computing nodes and login node) using FUSE (**F**ile System In **USE**rspace) which is a user-level filesystem. A caution is needed to ensure the /scratch filesystem is mounted because IME serves the role of a cache. The IME directory path is **/scratch\_ime,** in which all directories and file structures of the scratch (/scratch/$USER) filesystem can be checked when the first user accesses the directory (/scratch\_ime/$USER). The data does not exist in the actual IME device, and the data is cached by IME in /scratch when a job is executed using the burst buffer. To use the IME, the burst buffer project name (#PBS -P burst\_buffer) needs to be clarified in the job script. There are two methods for executing an application.

&#x20;

① /scratch\_ime, which is the mount point of IME, is designated as the I/O directory where the existing POSIX-based I/O can be executed without compilation. A user can submit a job using the conventional method but needs to set the data I/O path at the bottom of /scratch\_ime/$USER/.

1. ex) INPUT="/scratch\_ime/$USER/input.dat", OUTPUT="/scratch\_ime/$USER/output.dat"

```
#!/bin/sh
 #PBS -N burstbuffer
 #PBS -V #PBS -q normal        # all queues can be used 
 #PBS -A {PBS option name} # refer to the table of PBS option name per application 
 #PBS -P burst_buffer  # must be clarified for using burst buffer 
 #PBS -l select=2:ncpus=16:mpiprocs=16 
 #PBS -l walltime=05:00:00  
 
 cd $PBS_O_WORKDIR  
 
 OUTFILE=/scratch_ime/$USER/output.dat    
 
 # write execution commands related to the job (refer to the example in Chapter 4 "Job Execution through Scheduler" Section B.)
```

&#x20;

② To use MPI-IO based I/O, the mvapich2/2.3.1 module that supports IME must be used. Application programs need to be compiled again using the MPI library. The file or directory path needs to be designated using the IME protocol as shown in the example below.&#x20;

1. ex) OUTFILE=ime:///scratch/$USER/output.dat (refer to a sample job script below)

```
$ module load mvapich2/2.3.1
```

Load the mvapich2/2.3.1 module as above and write a job script as shown below.

&#x20;

```
#!/bin/sh
 #PBS -N mvapich2_ime 
 #PBS -V 
 #PBS -q normal            # all queues corresponding to KNL can be used (exclusive, normal, long, flat, debug) 
 #PBS -A {PBS option name} # refer to the table of PBS option name per application 
 #PBS -P burst_buffer     # Must be clarified for using burst buffer 
 #PBS -l select=2:ncpus=16:mpiprocs=16 
 #PBS -l walltime=5:00:00  
 
 cd $PBS_O_WORKDIR 
 TOTAL_CPUS=$(wc -l $PBS_NODEFILE | awk '{print $1}') 
 OUTFILE=ime:///scratch/$USER/output.dat   
 mpirun_rsh -np ${TOTAL_CPUS} -hostfile $PBS_NODEFILE ./a.out  
 or 
 mpirun -np ${TOTAL_CPUS} -hostfile $PBS_NODEFILE ./a.out   
```

&#x20;

\* Supported compiler: gcc/6.1.0, gcc/7.2.0, intel/17.0.5, intel/18.0.1, intel/18.0.3, intel/19.0.4, pgi/18.10

\* MPI-IO of IME is implemented using a separate ROMIO interface, but the ROMIO function that officially supports IME is included starting from the MVAPICH2/2.3.1 version. (OpenMPI is not supported)

\* Burst buffer IME can be used in all computing nodes of Nurion (SKL, KNL).

&#x20;

&#x20;

&#x20;For the data management of IME, the life cycle of the data shown in the figure below needs to be examined. IME data processing largely consists of prestage, prefetch, sync, and release steps, where IME-API(#ime-ctl) commands for each step are provided.

![](<../../../../.gitbook/assets/버스트버퍼(Burst Buffer) 사용법(1).png>)

| ime-ctl -i $INPUT\_FILE  | <p>Stage-in is executed for the job data with IME</p><p>(Data caching from /scratch to /scratch_ime)</p>            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| ime-ctl -r $OUTPUT\_FILE | <p>IME data is synchronized with the parallel filesystem<br>(data is transferred from /scratch_ime to /scratch)</p> |
| ime-ctl -p $TMP\_FILE    | <p>Purge data in IME</p><p>(Purge the data in /scratch_ime)</p>                                                     |
| ime-ctl -s $FILE         | Provides status information of the IME data                                                                         |

Detailed options can be checked through \* #ime-ctl --help

&#x20;

&#x20;

**◦ Data processing**

&#x20;The total capacity of IME is approximately 900 TB, which is automatically flushed in the /scratch filesystel or deleted according to the usage. In IME, a cache space is automatically secured according to the two threshold settings below:

① When the total capacity of newly created data (Dirty Data) is 45% or higher

② When the overall available space is 15% or below

![](<../../../../.gitbook/assets/버스트버퍼(Burst Buffer) 사용법(2).png>)

**◦ Caution**

&#x20;During the initial job execution, IME involves a step of caching the PFS data to the IME device and a load of flushing or synchronizing the cache data back into the PFS. Therefore, performance improvement can be expected for a massive amount of small I/O, which is relatively vulnerable in PFS (Lustre) and for applications with frequent checkpointing or high I/O execution frequency.

Moreover, IME (approximately 0.9 PB) has a relatively small capacity because it is used as a PFS cache (approximately 20 PB). Hence, data must be carefully managed because it can be deleted from a cache according to the threshold settings if the IME capacity becomes full during use.

&#x20;

**※ Caution: The given IME-API command must be used to delete the data that has been cached in the IME. Caution is especially necessary when the rm command is used to delete data from /scratch\_ime because the data saved in the actual /scratch is deleted.**
