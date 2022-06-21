# Running Jobs Through Scheduler (SLURM)

The Neuron system’s job scheduler uses SLURM. This chapter introduces the method that is used to submit jobs through SLURM and relevant commands. For information on how to write a job script when submitting a job to SLURM, refer to \[Appendix 1] and the example showing how to write a job script file. ****&#x20;

&#x20;**※ User jobs can only be submitted from /scratch.**

## A. Queue Configuration

&#x20;**** - Wall clock time: 2 days (48 h)

&#x20;**** - Exclusive node policy is applied to all queues (partitions).

&#x20;**** - Job queue (partitions)

&#x20; **◦ The partitions general users can use are as follows (October 2020):**

| <p>Queue name</p><p>(CPU type_GPUtype_<br>Number of GPUs)</p> | <p>Number<br>of<br>allocated<br>nodes</p>               | <p>Total<br>number<br>of<br>CPU</p><p>cores</p>   | Limit the number of jobs submitted \*             | Limit the resource usage \*\* | Note   |                                                  |   |
| ------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- | ----------------------------- | ------ | ------------------------------------------------ | - |
| <p>Maximum number<br>of<br>jobs submitted<br>per user</p>     | <p>Maximum number<br>of<br>running jobs<br>per user</p> | <p>Maximum number<br>of<br>nodes used per job</p> | <p>Maximum number<br>of<br>GPUs used per user</p> |                               |        |                                                  |   |
| ivy\_v100\_2                                                  | 21                                                      | 420                                               | 4                                                 | -                             | **16** | <p>Equipped<br>with 2<br>V100</p>                |   |
| cas\_v100\_2                                                  | 15                                                      | 600                                               | 4                                                 | -                             | **20** | <p>Equipped<br>with 2<br>V100</p>                |   |
| cas\_v100nv\_4                                                | 4                                                       | 160                                               | 2                                                 | -                             | **12** | <p>Equipped<br>with 4<br>V100</p><p>(NVlink)</p> |   |
| cas\_v100nv\_8                                                | 5                                                       | 160                                               | 2                                                 | -                             | **16** | <p>Equipped<br>with 8<br>V100</p><p>(NVlink)</p> |   |
| cas32c\_v100\_2                                               | 5                                                       | 160                                               | 2                                                 | -                             | **6**  | <p>Equipped<br>with 2<br>V100</p>                |   |
| skl                                                           | 9                                                       | 324                                               | 2                                                 | 2                             | 2      | -                                                |   |
| bigmem                                                        | 2                                                       | 96                                                | 1                                                 | 1                             | 1      | -                                                |   |
| amd                                                           | 2                                                       | 128                                               | 1                                                 | 1                             | 1      | -                                                |   |
| optane                                                        | 1                                                       | 24                                                | 1                                                 | 1                             | 1      | -                                                |   |

\*  ****  Limit the number of jobs submitted ****&#x20;

&#x20;   ****    - Maximum number of jobs submitted per user:    If a job is submitted after this maximum number has been reached, an error occurs during submission.

&#x20;   ****    - Maximum number of running jobs per user: If a job is submitted after this maximum number has been reached, the newly submitted job waits until a previous job is finished.

\*\* Limit resource usage ****&#x20;

&#x20;   ****    - Maximum number of nodes used per job: If a job is submitted after this maximum number has been reached, the newly submitted job is not executed. This is independent of the number of nodes used by the multiple jobs run by a user at any given time. ****&#x20;

&#x20;   ****    - Maximum number of GPUs per user: This setting limits the total number of GPUs that can be used by a user. If this maximum number is reached, a newly submitted job has to wait until a previous job is finished. This setting limits the number of GPUs used by multiple jobs that are run by a user at any given time.

**※ The node configuration may be adjusted during system operation according to the system load.**

&#x20;

\- Node configuration

| Queue name      | Number of allocated nodes | Node list      |
| --------------- | ------------------------- | -------------- |
| ivy\_v100\_2    | 21                        | gpu\[08-28]    |
| cas\_v100\_2    | 15                        | gpu\[30-44]    |
| cas\_v100nv\_4  | 4                         | gpu\[45-48]    |
| cas\_v100nv\_8  | 5                         | gpu\[49-53]    |
| cas32c\_v100\_2 | 5                         | gpu\[54-58]    |
| skl             | 10                        | skl\[01-10]    |
| bigmem          | 2                         | bigmem\[01-02] |
| amd             | 2                         | amd\[01-02]    |
| optane          | 1                         | optane01       |

&#x20;

&#x20;

## B. Submitting and Monitoring Jobs

1\) Summary of the basic commands

| **Command**                  | Description                                  |
| ---------------------------- | -------------------------------------------- |
| $ sbatch \[option..] script  | Submit a job                                 |
| $ scancel Job ID             | Remove a job                                 |
| $ squeue                     | Check job status                             |
| $ smap                       | Check job status and node status graphically |
| $ sinfo \[option..]          | Check node information                       |

&#x20;

※ You can use the “sinfo –help” command to check the “sinfo” command options.

&#x20;

**※ For the purpose of collecting data to enhance the convenience and benefits of the Neuron system users, users are required to fill out the information about the program they are using through the SBATCH option shown below. That is, they must submit a job after entering the --comment option of the SBATCH command according to the application they are using by referring to the table below.**

**※ If you are using an application for deep learning or machine learning, please specify the application type, such as TensorFlow, Caffe, R, and PyTorch.**

**※ Application categories are added based on the user requests, which are collected periodically. If you would like to have an application added, please make a request to add the application by sending an email to** [**consult@ksc.re.kr**](mailto:consult@ksc.re.kr%EB%A1%9C)**.**

&#x20;

**\[Table of SBATCH option name per application]**

| **Application type** | **SBATCH option name** | **Application type** | **SBATCH option name** |
| -------------------- | ---------------------- | -------------------- | ---------------------- |
| Charmm               | charmm                 | LAMMPS               | lammps                 |
| Gaussian             | gaussian               | NAMD                 | namd                   |
| OpenFoam             | openfoam               | Quantum Espresso     | qe                     |
| WRF                  | wrf                    | SIESTA               | siesta                 |
| in-house code        | inhouse                | Tensorflow           | tensorflow             |
| PYTHON               | python                 | Caffe                | caffe                  |
| R                    | R                      | Pytorch              | pytorch                |
| VASP                 | vasp                   | Sklearn              | sklearn                |
| Gromacs              | gromacs                | Other applications   | etc                    |

&#x20;

&#x20;

**2) Batch job submission**&#x20;

&#x20; ****  Submit a job by using the **** sbatch command as in “sbatch {script file}”.

```
$ sbatch mpi.sh
```

※ To submit a job, use the mpi.sh file written as a sample job script file.

&#x20;

\- Check job progress

&#x20;You can connect to the allocated node to check the job progress.

① Use the squeue command to check the node name (NODELIST) to which the running job is assigned.

② Use the ssh command to connect to the allocated node.

```
$ ssh
```

&#x20;

③ Connect to the compute node and use either the top or nvidia-smi command to check the job progress.

※ Example of monitoring the GPU utilization every 2 s

```
$ nvidia-smi -l 2
```

&#x20;

&#x20;

\- Example of writing a job script file

&#x20;**** A job script file needs to be created using SLURM keywords to run a batch job in SLURM. ****&#x20;

※ Refer to “\[Appendix 1] Main Keywords for Job Scripts”.

※ Refer to the KISTI supercomputing blog ([http://blog.ksc.re.kr/127](http://blog.ksc.re.kr/127)) for information on how to use the machine-learning framework conda.

&#x20;

\- SLURM keywords

| Keyword            | Description                                   |
| ------------------ | --------------------------------------------- |
| #SBATCH –J         | Specify the job name                          |
| #SBATCH --time     | Specify the maximum time to run the job       |
| #SBATCH –o         | Specify the file name of the job log          |
| #SBATCH –e         | Specify the file name of the error log        |
| #SBATCH –p         | Specify the partition to be used              |
| #SBATCH --comment  | Specify the application to be used            |
| #SBATCH -–nodelist | Specify the node on which the job will be run |

&#x20;

※ Example of the SBATCH --nodelist keyword

```
#SBATCH --nodelist=bigmem02
```

&#x20;

&#x20;

**■ CPU Serial Program**&#x20;

```
#!/bin/sh#SBATCH -J Serial_cpu_job#SBATCH -p skl#SBATCH -N 1#SBATCH -n 1#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --comment etc #Refer to the Table of SBATCH option name per application export OMP_NUM_THREADS=1 module purgemodule load intel/18.0.2 ./test.exe exit 0
```

※ Example of using 1 node sequentially

&#x20;

**■ CPU OpenMP Program**&#x20;

```
#!/bin/sh#SBATCH -J OpenMP_cpu_job#SBATCH -p skl#SBATCH -N 1#SBATCH -n 1#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --comment etc # Refer to the Table of SBATCH option name per application export OMP_NUM_THREADS=10module purgemodule load intel/18.0.2 ./test_omp.exe exit 0
```

※ Example of using 1 node with 10 threads per node

&#x20;

**■ CPU MPI Program**&#x20;

```
#!/bin/sh#SBATCH -J MPI_cpu_job#SBATCH -p skl#SBATCH -N 2 # number of node#SBATCH -n 8 # total process#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --comment etc # Refer to the Table of SBATCH option name per applicationmodule purgemodule load intel/18.0.2 mpi/impi-18.0.2 srun ./test_mpi.exe
```

&#x20;**** ※ Example of using 2 nodes and 4 processes per node (total of 8 MPI processes)

&#x20;

**■ CPU Hybrid (OpenMP+MPI) Program**&#x20;

```
#!/bin/sh#SBATCH -J hybrid_cpu_job#SBATCH -p skl#SBATCH -N 1 # number of node#SBATCH -n 2#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --comment etc #Refer to the Table of SBATCH option name per application module purgemodule load intel/18.0.2 mpi/impi-18.0.2 export OMP_NUM_THREADS=10 srun ./test_mpi.exe
```

※ Example of using 1 node, 2 processes per node, and 10 threads per process (total of 2 MPI processes, 20 OpenMP threads)

&#x20;

**■ GPU Serial Program**&#x20;

```
#!/bin/sh#SBATCH -J Serial_gpu_job#SBATCH -p ivy_v100_2#SBATCH -N 1#SBATCH -n 1#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --gres=gpu:2#SBATCH --comment etc #Refer to the Table of SBATCH option name per application export OMP_NUM_THREADS=1 module purgemodule load intel/18.0.2 cuda/10.0 ./test.exe exit 0
```

※ Example of using 1 node sequentially

&#x20;

**■ GPU OpenMP Program**&#x20;

```
#!/bin/sh#SBATCH -J openmp_gpu_job#SBATCH -p ivy_v100_2#SBATCH -N 1 # number of nodes#SBATCH -n 1#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --gres=gpu:2 # using 2 gpus#SBATCH --comment etc #Refer to the Table of SBATCH option name per application  export OMP_NUM_THREADS=10module purgemodule load intel/18.0.2 cuda/10.0 ./test_omp.exe exit 0
```

※ Example of using 1 node, 10 threads per node, and 2 GPUs

&#x20;

**■ GPU MPI Program**&#x20;

```
#!/bin/sh#SBATCH -J mpi_gpu_job#SBATCH -p ivy_v100_2#SBATCH -N 2 # number of nodes#SBATCH -n 8 # total process#SBATCH -o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --time=01:00:00#SBATCH --gres=gpu:2 # using 2 gpus#SBATCH --comment etc #Refer to the Table of SBATCH option name per application module purgemodule load intel/18.0.2 cuda/10.0 cudampi/mvapich2-2.3 srun ./test_mpi.exe
```

※ Example of using 2 nodes, 4 processes per node (total of 8 MPI processes), and 2 GPUs

&#x20;

**■ An example of using GPU singularity**

```
1) Single node#!/bin/sh#SBATCH -J singularity#SBATCH --time=1:00:00 #SBATCH -p ivy_v100_2#SBATCH --comment tensorflow #Refer to the Table of SBATCH option name per application#SBATCH -N 1 # number of nodes#SBATCH -n 2 #SBATCH –o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --gres=gpu:2 module load singularity/3.6.4  singularity run —nv /apps/applications/singularity_images/ngc/tensorflow:20.09-tf1-py3.sif python test.py  2-1) Multi-node (using horovod)#!/bin/sh#SBATCH -J singularity#SBATCH --time=1:00:00 #SBATCH -p ivy_v100_2#SBATCH --comment tensorflow #Refer to the Table of SBATCH option name per application#SBATCH -N 2 # number of nodes#SBATCH –n 4 #SBATCH –o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --gres=gpu:2 export Base=/scratch/ID/work_dirmodule load singularity/3.6.4 gcc/4.8.5 mpi/openmpi-3.1.5 srun singularity run —nv /apps/applications/singularity_images/ngc/tensorflow:20.09-tf1-py3.sif \python $Base/horovod/examples/keras/keras_imagenet_resnet50.py   2-2) Multi-node (using horovod): When the module is loaded, the specified singularity container is automatically run when the user program is executed.#!/bin/sh#SBATCH -J singularity#SBATCH --time=1:00:00 #SBATCH -p ivy_v100_2#SBATCH --comment tensorflow #Refer to the Table of SBATCH option name per application#SBATCH -N 2 # number of nodes#SBATCH –n 4 #SBATCH –o %x_%j.out#SBATCH -e %x_%j.err#SBATCH --gres=gpu:2 export Base=/scratch/ID/work_dirmodule load singularity/3.6.4 ngc/tensorflow:20.09-tf1-py3  # tensorflow:20.09-tf1-py3.sif After the container is run automatically, the resnet50 model is trained in parallel on multiple nodes.mpirun_wrapper python $Base/horovod/examples/keras/keras_imagenet_resnet50.py
```

※ Container images that support deep learning frameworks, such as TensorFlow, Caffe, and PyTorch, can be accessed in the /apps/applications/singularity\_images and /apps/applications/singularity\_images/ngc directories.

※ Refer to "\[Appendix 3] Method for Using Singularity Container Images - Method for running the user program from the module-based NGC container" for the list of deep learning and HPC application modules that support the automatic execution of the singularity container.

&#x20;

\[Example] ****&#x20;

You can use the container images that support deep learning frameworks, such as TensorFlow, Caffe, and PyTorch, by copying them from the /apps/applications/singularity\_images directory to the user work directory.

Use the following example if the image file is “/home01/userID/tensorflow-1.13.0-py3.simg”.

```
singularity exec /home01/userID/tensorflow-1.13.0-py3.simg python test.py
```

&#x20;****&#x20;

**3) Interactive job submission**

\- Resource allocation

\* Explanation: Two GPU nodes (each node having 2 cores and 2 GPUs) of the ivy\_v100\_2 partition are used for the interactive purpose.

```
$ salloc --partition=ivy_v100_2 -N 2 -n 4 --tasks-per-node=2 --gres=gpu:2 --comment={SBATCH option name} # Refer to the Table of SBATCH option name per application 
```

**※ If there is no keyboard input for more than 2 h, the job is terminated owing to a timeout, and resources are released. The walltime of the interactive job is set to 12 h.**

&#x20;

\- Job execution

```
$ srun ./(executable file) (run option)
```

&#x20;

\- Connect to the first node (head node) among the allocated compute nodes

```
$ srun --pty bash
```

**※ If there is no keyboard input for more than 2 h, the job is terminated owing to a timeout, and resources are released.**

**※ After connecting to the head node, it is not possible to submit jobs using the srun or mpirun command. However, it is possible to submit jobs after exiting from the head node (exit).**

&#x20;

\- Exit from the connected node, or cancel the resource allocation.

```
$ exit
```

&#x20;

\- Delete a job using a command.

```
$ scancel [Job_ID]
```

※ Job ID can be checked using the squeue command.

&#x20;

&#x20;

**4) Job monitoring**

**- Check the partition status**

&#x20;   ****    Use the **** sinfo command to check **** the partition status.

&#x20;                                                                ****                                                                 (As of August 2019) ****&#x20;

```
$ sinfoPARTITION         AVAIL  TIMELIMIT  NODES  STATE NODELISTivy_k40_2          up    1-00:00:00     8   idle gpu[01-08]ivy_v100_2         up    1-00:00:00    12   idle gpu[09-19]ivy_v100_1         up    1-00:00:00     9   idle gpu[20-30]cas_v100_2         up    1-00:00:00    15   idle gpu[30-44]cas_v100nv_4       up    1-00:00:00     4   idle gpu[45-48] skl                up    1-00:00:00     9   idleskl[01-09]bigmem             up    1-00:00:00     2   idle bigmem[01-02]
```

&#x20; **※ The node configuration may be adjusted during system operation according to the system load.**

&#x20; ****  ※ PARTITION : the name of the partition set in the current SLURM

&#x20;    ****     AVAIL : partition status (up or down)

&#x20;    ****     TIMELIMIT : wall clock time

&#x20;    ****     NODES : the number of nodes

&#x20;    ****     STATE : node status (alloc-resource is being used/Idle-resource is available)

&#x20;    ****     NODELIST : node list

&#x20;

&#x20;**- Detailed information per node**&#x20;

&#x20; Check the detailed information by using the "-Nel" option after the sinfo command. ****&#x20;

```
$ sinfo -NelThu Apr  4 18:33:52 2019NODELIST NODES PARTITION   STATE CPUS S:C:T  MEMORY TMP_DISK WEIGHT   AVAIL_FE  REASON gpu01      1  ivy_k40_2     idle  20  2:10:1 128000     0       1     TeslaK40   nonegpu02      1  ivy_k40_2     idle  20  2:10:1 128000     0       1     TeslaK40   nonegpu03      1  ivy_k40_1     idle  20  2:10:1 128000     0       1     TeslaK40   none- -  Subsequent information is omitted - -
```

&#x20;

**- Check job status**

&#x20; Check the list and status of the jobs by using the squeue command.

```
$ squeue  JOBID PARTITION     NAME         USER   ST     TIME     NODES.      NODELIST(REASON)  760   ivy_k40_2    gpu_burn     userid   R     0:00       10          gpu01  761   ivy_v100_2   gpu_burn     userid   R     0:00       10          gpu02  762   ivy_v100_1   gpu_burn     userid   R     0:00       10          gpu03
```

&#x20;

**- Check the job status and node status graphically**

```
$ smap
```

&#x20;

**- Check the detailed information on the submitted job**

```
$ scontrol show job [Job ID]
```

&#x20;\
\


## C. Controlling Jobs

**- Delete a job (cancel)**

&#x20;Use the **** scancel command, as in “scancel \[Job\_ID]," to delete a job.

&#x20;**** Job\_ID can be checked by using the squeue command.

```
$ scancel 761
```

&#x20;\
\


## D. Compilation, Debugging, and Job Submission Location per Partition

&#x20;**** - With the user account, ssh connection is not possible except for the nodes specified below.

&#x20;**** - Because the same home and scratch directories are mounted on all nodes, it is possible to submit jobs for all partitions from the login node (glogin\[01-02]).

| Queue name     | Compilation           | Debugging            |
| -------------- | --------------------- | -------------------- |
| ivy\_v100\_2   | glogin\[01-02]        |                      |
| cas\_v100\_2   |                       |                      |
| cas\_v100nv\_4 |                       |                      |
| skl,bigmem     | glogin\[01-02]        | SLURM Interative job |
| amd,optane     | SLURM Interactive job |                      |

&#x20;

&#x20;**** - It is possible to perform compilation and debugging in all partitions using the SLURM Interactive job function when necessary.

&#x20;
