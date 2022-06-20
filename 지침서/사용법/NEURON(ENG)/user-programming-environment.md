# User Programming Environment

## A. Programming Tool Installation Status&#x20;

| **Category**                        | **Modules**                                                                                                                                                                                                             |                                                                     |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Compiler                            | <p>∙ gcc/4.8.5</p><p>∙ gcc/8.3.0</p><p>∙ Intel/18.0.2</p><p>∙ pgi/19.1</p>                                                                                                                                              |                                                                     |
| Compiler-dependent library          | <p>∙ hdf4/4.2.13</p><p>∙ hdf5/1.10.2</p><p>∙ lapack/3.7.0</p><p>∙ ncl/6.5.0</p><p>∙ netcdf/4.6.1</p>                                                                                                                    |                                                                     |
| CUDA library                        | ∙ CUDA/10.0                                                                                                                                                                                                             |                                                                     |
| MPI library                         | CUDA                                                                                                                                                                                                                    | <p>∙ MVAPICH2/2.3  </p><p>∙ OpenMPI/3.1.0</p><p>∙ OpenMPI/3.1.5</p> |
| Non CUDA                            | <p>∙ impi/18.0.2</p><p>∙ MVAPICH2/2.3  </p><p>∙ OpenMPI/3.1.0</p><p>∙ OpenMPI/3.1.5</p>                                                                                                                                 |                                                                     |
| MPI-dependent library               | <p>∙ fftw_mpi/2.1.5</p><p>∙ fftw_mpi/3.3.7</p>                                                                                                                                                                          |                                                                     |
| Commercial software                 | <p>∙ gaussian/g16</p><p>∙ gaussian/g16.b01</p><p>∙ gaussian/g16.c01</p>                                                                                                                                                 |                                                                     |
| Application software                | <p>∙ cmake/3.12.3</p><p>∙ java_openjdk/11.0.1</p><p>∙ python/2.7.15</p><p>∙ python/3.7.1</p><p>∙ namd/2.12</p><p>∙ qe/6.4.1_k40</p><p>∙ qe/6.4.1_v100</p><p>∙ gromacs/2016.4</p><p>∙ lammps/16Mar18</p><p>∙ R/3.5.0</p> |                                                                     |
| Virtualization module               | <p>∙ singularity/3.1.0</p><p>∙ singularity/3.6.4</p>                                                                                                                                                                    |                                                                     |
| Machine-learning framework software | <p>∙ Caffe_gpu/1.0</p><p>∙ Tensorflow/1.13</p><p>∙ Pytorch/1.0</p>                                                                                                                                                      |                                                                     |

&#x20;

※ We recommend using the Anaconda environment for the artificial intelligence framework of the Neuron system. We also plan to provide an environment that uses Singularity to run container images based on user requirements.

※ Using Singularity: Refer to “\[Appendix 3] Method for Using Singularity Container Images”

**※ Non-CUDA MPI libraries must be employed to use nodes that are not equipped with GPUs.**

&#x20;\
\


## B. How to Use the Compilers

#### **1) Compiler and MPI environment configuration (modules)**

#### **(1) Module-related basic commands**

&#x20;ㅇ Print a list of available modules

You can check a list of available modules, such as compilers and libraries.

```
$ module availor$ module av
```

&#x20;

&#x20;ㅇ Add a module to be used

You can add modules that you plan to use, such as compilers and libraries.

Modules that will be used can be added simultaneously.

```
$ module load [module name] [module name] [module name] ...or$ module add [module name] [module name] [module name] ...
```

&#x20;

&#x20;ㅇ Delete used modules

You can remove unnecessary modules. Several modules can be deleted simultaneously.

```
$ module unload [module name] [module name] [module name] ...or$ module rm [module name] [module name] [module name] ...
```

&#x20;

&#x20;ㅇ Print a list of used modules

You can check the list of modules that are currently configured.

```
$ module listor$ module li
```

&#x20;

&#x20;ㅇ Delete all used modules simultaneously.

```
$ module purge
```

&#x20;

&#x20;ㅇ Check the module installation path.

```
$ module show [module name]
```

&#x20;

&#x20;

#### 2) Compiling sequential programs

A sequential program is a program that does not consider the parallel program environment. That is, it is a program that does not use a parallel program interface, such as OpenMP or MPI. A sequential program is run using only one processor in one node. Compiler-specific options used for compiling sequential programs are also used when compiling parallel programs. Hence, reference should be made to these options even if you are not interested in sequential programs.

&#x20;

#### ① Intel compiler

To use an Intel compiler, add the required version of the Intel compiler module. Available modules can be checked using the “module avail” command.

```
$ module load intel/18.0.2
```

※ Check available versions by referring to the programming tool installation status table.

&#x20;

#### ■ Compiler types

| Compiler   | Program | Source file extension                                  |
| ---------- | ------- | ------------------------------------------------------ |
| icc / icpc | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| ifort      | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

#### ■ Intel compiler usage example

The following is an example of compiling a test sample file using an Intel compiler to generate a test.exe executable file.

```
$ module load intel/18.0.2$ icc -o test.exe test.cor$ ifort -o test.exe test.f90$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

&#x20;

**② GNU compiler**

To use a GNU compiler, add the required version of the GNU compiler module. Available modules can be checked using the “module avail” command.

```
$ module load gcc/8.3.0
```

※ Check available versions by referring to the programming tool installation status table.

※ You must use version "gcc/4.8.5" or higher.

&#x20;

#### ■ Compiler types

| Compiler  | Program | Source file extension                                  |
| --------- | ------- | ------------------------------------------------------ |
| gcc / g++ | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| gfortran  | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

#### ■ GNU compiler usage example

The following is an example of compiling a test sample file using a GNU compiler to generate a test.exe executable file.

```
$ module load gcc/8.3.0$ gcc -o test.exe test.cor$ gfortran -o test.exe test.f90$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

&#x20;

#### ③ PGI compiler

To use a PGI compiler, add the required version of the PGI compiler module. Available modules can be checked using the “module avail” command.

```
$ module load pgi/19.1
```

※ Check available versions by referring to the programming tool installation status table.

&#x20;

**■ Compiler types**

| Compiler     | Program | Source file extension                                  |
| ------------ | ------- | ------------------------------------------------------ |
| pgcc / pgc++ | C / C++ | .C, .cc, .cpp, .cxx,.c++                               |
| pgfortran    | F77/F90 | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

**■ PGI compiler usage example**

The following is an example of compiling a test sample file using a PGI compiler to generate a test.exe executable file.

```
$ module load pgi/19.1$ pgcc -o test.exe test.cor$ pgfortran -o test.exe test.f90$ ./test.exe
```

※ You can copy a test sample file for job submission from /apps/shell/job\_examples and use it.

&#x20;

#### 3) Compiling parallel programs

#### (1) OpenMP compiling

OpenMP is a technique that has been developed to utilize multi-threading by only using compiler directives. The compiler used to compile a parallel program using OpenMP is the same as the compiler used for a sequential program. Parallel compiling can be performed by adding compiler options, and most compilers currently support the OpenMP directives.

| Compiler option          | Program           | Option   |
| ------------------------ | ----------------- | -------- |
| icc / icpc / ifort       | C / C++ / F77/F90 | -qopenmp |
| gcc / g++ / gfortran     | C / C++ / F77/F90 | -fopenmp |
| pgcc / pgc++ / pgfortran | C / C++ / F77/F90 | -mp      |

&#x20;

#### ① Example of compiling an OpenMP program (Intel compiler)

The following is an example of compiling a test\_omp sample file using openMP with an Intel compiler to generate a test\_omp.exe executable file.

```
$ module load intel/18.0.2$ icc -o test_omp.exe -qopenmp test_omp.cor$ ifort -o test_omp.exe -qopenmp test_omp.f90$ ./test_omp.exe
```

&#x20;

#### ② Example of compiling an OpenMP program (GNU compiler)

The following is an example of compiling a test\_omp sample file using openMP with a GNU compiler to generate a test\_omp.exe executable file.

```
$ module load gcc/8.3.0$ gcc -o test_omp.exe -fopenmp test_omp.cor$ gfortran -o test_omp.exe -fopenmp test_omp.f90$ ./test_omp.exe
```

&#x20;

#### ③ Example of compiling an OpenMP program (PGI compiler)

The following is an example of compiling a test\_omp sample file using openMP with a PGI compiler to generate a test\_omp.exe executable file.

```
$ module load pgi/19.1$ pgcc -o test_omp.exe -mp test_omp.cor$ pgfortran -o test_omp.exe -mp test_omp.f90$ ./test_omp.exe
```

&#x20;

#### (2) MPI compiling

The user can run the MPI commands in the following table. These commands are a sort of wrapper, and the compiler that is specified by the .bashrc file compiles the source file.

&#x20;

| **Category**  | **Intel** | **GNU**  | **PGI**   |
| ------------- | --------- | -------- | --------- |
| Fortran       | ifort     | gfortran | pgfortran |
| Fortran + MPI | mpiifort  | mpif90   | mpif90    |
| C             | icc       | gcc      | pgcc      |
| C + MPI       | mpiicc    | mpicc    | mpicc     |
| C++           | icpc      | g++      | pgc++     |
| C++ + MPI     | mpiicpc   | mpicxx   | mpicxx    |

Even if compiling is performed using mpicc, it is necessary to use the options that correspond to the original compiler being wrapped.

&#x20;

#### ① Example of compiling an MPI program (Intel compiler)

The following is an example of compiling a test\_mpi sample file using MPI with an Intel compiler to generate a test\_mpi.exe executable file.

```
$ module load intel/18.0.2 impi/18.0.2$ mpiicc -o test_mpi.exe test_mpi.cor$ mpiifort -o test_mpi.exe test_mpi.f90$  srun ./test_mpi.exe
```

&#x20;

#### ② Example of compiling an MPI program (GNU compiler)

The following is an example of compiling a test\_mpi sample file using MPI with a GNU compiler to generate a test\_mpi.exe executable file.

```
$ module load gcc/8.3.0 openmpi/3.1.5$ mpicc -o test_mpi.exe test_mpi.c$ or $ mpif90 -o test_mpi.exe test_mpi.f90$ srun ./test_mpi.exe
```

&#x20;

#### ③ Example of compiling an MPI program (PGI compiler)

The following is an example of compiling a test\_mpi sample file using MPI with a PGI compiler to generate a test\_mpi.exe executable file.

```
$ module load pgi/19.1 openmpi/3.1.5$ mpicc -o test_mpi.exe test_mpi.cor$ mpifort -o test_mpi.exe test_mpi.f90$ srun ./test_mpi.exe
```

\
