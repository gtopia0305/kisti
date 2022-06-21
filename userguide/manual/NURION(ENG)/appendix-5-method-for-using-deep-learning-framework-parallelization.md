# \[Appendix 5] Method for Using Deep Learning Framework Parallelization

## A. Method for Using Horovod in TensorFlow&#x20;

&#x20;

It is possible to perform parallelization by using Horovod with TensorFlow when using CPUs in multiple nodes. As shown in the example below, Horovod can be used with TensorFlow by adding code for using Horovod. Both TensorFlow and all Keras APIs that can be used in TensorFlow can be used with Horovod. We first introduce the method for using Horovod in TensorFlow.\
(Example: MNIST Dataset and LeNet-5 CNN structure)

&#x20;

※ Refer to the official Horovod guide for detailed information on how to use Horovod in TensorFlow\
([https://github.com/horovod/horovod#usage](https://github.com/horovod/horovod#usage))

&#x20;

**◦ Import statement to use Horovod in TensorFlow and the Horovod initialization in the main function**

```
import horovod.tensorflow as hvd
...
hvd.init()
```

※ horovod.tensorflow: a module for using Horovod with TensorFlow

※ Horovod is initialized, so it can be used.

&#x20;

**◦ Dataset setting for using Horovod in the main function**

```
(x_train, y_train), (x_test, y_test) = \
keras.datasets.mnist.load_data('MNIST-data-%d' % hvd.rank())
```

※ The dataset to be accessed for each job is set and created according to the Horovod rank.

&#x20;****&#x20;

**◦ Set the Horovod-related settings, broadcast, and the number of training epochs for the optimizer in the main function**

```
opt = tf.train.AdamOptimizer(0.001 * hvd.size())
opt = hvd.DistributedOptimizer(opt)
global_step = tf.train.get_or_create_global_step()
train_op = opt.minimize(loss, global_step=global_step)
hooks = [hvd.BroadcastGlobalVariablesHook(0),
                 tf.train.StopAtStepHook(last_step=20000 // hvd.size()), ... ]
```

※ Apply Horovod-related settings to the optimizer and use broadcast to convey them to each job.

※ Set the training process step of each job according to the number of Horovod jobs.

&#x20;

**◦ Parallel processing settings for Inter operation and Intra operation**

```
config = tf.ConfigProto()
config.intra_op_parallelism_threads = int(os.environ[‘OMP_NUM_THREADS’])
config.inter_op_parallelism_threads = 2
```

※ config.intra\_op\_parallelism\_threads: is used to set the number of threads to be used in computation jobs and is applied by loading the OMP\_NUM\_THREADS set in the job script. (In this example, OMP\_NUM\_THREADS is set to 32.)

※ config.intra\_op\_parallelism\_threads: is the number of threads executing the TensorFlow jobs simultaneously. If this number is set to 2, two jobs are executed in parallel, as shown in the example.

&#x20;

**◦ Set checkpoint for the rank 0 job**

```
checkpoint_dir = './checkpoints' if hvd.rank() == 0 else None
...
with tf.train.MonitoredTrainingSession(checkpoint_dir=checkpoint_dir,
hooks=hooks,
config=config) as mon_sess:
```

※ The job for saving or retrieving a checkpoint must be performed by a single process, so it is set to rank 0.

&#x20;

&#x20;

## B. Method for Using Multiple Nodes in Intel Caffe

&#x20;

Horovod does not officially support multi-node parallelization in Caffe. However, parallel processing can be run by using Intel Caffe, which was optimized for KNL and developed by Intel. In the case of Intel Caffe, all tasks for parallel processing are applied in the code development process. Hence, deploy.prototxt, solver.prototxt, and train\_val.prototxt, which have been developed in Caffe, can be used as is.

&#x20;

※ Refer to the official Intel Caffe guide for detailed information on how to use Intel Caffe\
([https://github.com/intel/caffe/wiki/Multinode-guide](https://github.com/intel/caffe/wiki/Multinode-guide))

&#x20;

When parallel processing is performed on the Caffe code that has been modified by a deep learning developer, the corresponding part in the Intel Caffe source code needs to be updated, compiled, and then executed.

&#x20;

**◦ Method for performing parallel processing in Intel Caffe (job script example)**

```
#!/bin/sh
#PBS -N test
#PBS -V
#PBS -l select=4:ncpus=68:mpiprocs=1:ompthreads=68
#PBS -q normal
#PBS -l walltime=04:00:00
#PBS -A caffe

cd $PBS_O_WORKDIR

module purge
module load conda/intel_caffe_1.1.5
source /apps/applications/miniconda3/envs/intel_caffe/mlsl_2018.3.008/intel64/bin/mlslvars.sh

export KMP_AFFINITY=verbose,granularity=fine,compact=1
export KMP_BLOCKTIME=1
export KMP_SETTINGS=1
export OMP_NUM_THREADS=60
mpirun -PSM2 -prepend-rank caffe train \
--solver ./models/intel_optimized_models/multinode/alexnet_4nodes/solver.prototxt
# or
./scripts/run_intelcaffe.sh --hostfile $PBS_NODEFILE \
--caffe_bin /apps/applications/miniconda3/envs/intel_caffe/bin/caffe \
--solver models/intel_optimized_models/multinode/alexnet_4nodes/solver.prototxt \
--network opa --ppn 1 --num_omp_threads 60
exit 0
```

&#x20;

※ PPN: is an abbreviation of processes per node and indicates the number of jobs per node (default value: 1).

※ Network option: is set to Intel Omni-Path Architecture (OPA).

※ It can be performed in the same manner as the existing Caffe method by running MPI without using the script.
