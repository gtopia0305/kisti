---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > User Programming Environment > B. How to Use
  Compiler
---

# B. How to Use Compiler

1\) Compiler and MPI configuration settings (modules)

(1) Default required module

\- One corresponding module must be added to the computing node being used.

| **Computing node to be used** | **KNL**        | **SKL**            |
| ----------------------------- | -------------- | ------------------ |
| Module name                   | craype-mic-knl | craype-x86-skylake |

&#x20;

```
$ module load craype-mic-knl 
or
$ module load craype-x86-skylake
```

&#x20;

(2) Basic commands related to the module

▶ Print a list of available modules

A list of modules that are available such as compiler and library can be checked.

```
$ module avail 
or  
$ module av
```

&#x20;

▶ Add a module to be used

Modules to be used such as compiler and library can be added.

All modules to be used can be added at once.

```
$ module load [module name] [module name] [module name] … or $ module add [module name] [module name] [module name] …
```

&#x20;

▶ Delete modules being used

Remove the modules that are no longer needed. Multiple modules can be deleted at once.

```
$ module unload [module name] [module name] [module name] ... 
or 
$ module rm [module name] [module name] [module name] ...
```

▶ Print a list of modules being used.

A list of currently set modules can be checked.

```
$ module list 
or 
$ module li
```

&#x20;

▶ Purge all modules being used

```
$ module purge
```

※ In this case, the default required modules are also deleted at once; those modules need to be added again when reusing them.

&#x20;

2\) Compiling sequential programs

A sequential program refers to a program in which a parallel program environment is not considered. Specifically, it is a program that does not use a parallel program interface such as OpenMP or MPI and is not used where the program is executed using one processor in one node. Options per compiler used for compiling a sequential program are also used when compiling a parallel program; thus, it is recommended to reference them even if a sequential program is not of interest.

&#x20;

**① Intel compiler**

A version of an Intel compiler module required for using the Intel compiler should be added. Available modules can be checked with the command module avail.

```
$ module load intel/18.0.3
```

※ Check the available version by referring to the programming tool installation status table.

&#x20;

■ Compiler type

| **Compiler** | **program** | **Source extensions**                                  |
| ------------ | ----------- | ------------------------------------------------------ |
| icc / icpc   | C / C++     | .C, .cc, .cpp, .cxx,.c++                               |
| ifort        | F77/F90     | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

■ Compiler option

| **Compiler option**                         | **Description**                                                                                                                                                                   |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -O\[1\|2\|3]                                | Object optimization. Optimization level for numbers.                                                                                                                              |
| -ip, ipo                                    | Optimization between procedures                                                                                                                                                   |
| -qopt\_report=\[0\|1\|2\|3\|4]              | Adjusts the amount of vector diagnostic information                                                                                                                               |
| <p>-xCORE-AVX512<br>-xMIC-AVX512</p>        | <p>Supports CPU with a 512 bit register (when the SKL node is used for computing)<br>Supports MIC with a 512 bit register (when the KNL node is used for computing)</p>           |
| -fast                                       | -O3 -ipo -no-prec-div -static, -fp-model fast=2 macro                                                                                                                             |
| <p>-static/-static-intel/<br>-i_static</p>  | Does not allow shared libraries to be linked                                                                                                                                      |
| <p>-shared/-shared-intel/<br>-i_dynamic</p> | Allows shared libraries to be linked                                                                                                                                              |
| -g -fp                                      | Generates debugging information                                                                                                                                                   |
| -qopenmp                                    | Uses OpenMP-based multi-thread code                                                                                                                                               |
| -openmp\_report=\[0\|1\|2]                  | Adjusts OpenMP parallelization diagnostic level                                                                                                                                   |
| <p>-ax<br>-axS</p>                          | <p>Generates code optimized for a specific processor<br>Generates specialized code utilizing the SIMD Extensions4 (SSE4) vectorizing compiler and media acceleration commands</p> |
| -tcheck                                     | Activates the analysis of thread-based programs                                                                                                                                   |
| -pthread                                    | Adds the pthread library to receive multi-threading support                                                                                                                       |
| -msse<3,4.1>,-msse3                         | Supports Streaming SIMD Extensions 3                                                                                                                                              |
| -fPIC,fpic                                  | Compiles to be position independent code (PIC)                                                                                                                                    |
| -p                                          | Generates profiling information (gmon.out)                                                                                                                                        |
| -unroll                                     | Unroll activation, is the maximum number (only supports 64 bit)                                                                                                                   |
| -mcmodel medium                             | Used when a memory allocation of 2 GB or higher is required                                                                                                                       |
| -help                                       | Outputs a list of options                                                                                                                                                         |

&#x20;

■ Example of using Intel compiler

The following is an example of creating an execution file test.exe by compiling a test sample file with the Intel compiler in the **KNL computing node**.

```
$ module load craype-mic-knl intel/18.0.3 
$ icc –o test.exe –O3 –fPIC –xMIC-AVX512 test.c 
or
$ ifort -o test.exe -O3 -fPIC -xMIC-AVX512 test.f90 
$ ./test.exe
```

※ Copy the test sample file for job submission in /apps/shell/home/job\_examples

&#x20;

■ Recommended options

| **Computing node** | **Recommended options**   |
| ------------------ | ------------------------- |
| SKL                | -O3 –fPIC –xCORE-AVX512   |
| KNL                | -O3 -fPIC -xMIC-AVX512    |
| SKL & KNL          | -O3 –fPIC -xCOMMON-AVX512 |

&#x20;

**② GNU compiler**

Add a GNU compiler module required for using the GNU compiler. Available modules can be checked with the command module avail.

```
$ module load gcc/7.2.0
```

※ Check the available version by referring to the programming tool installation status table.

※ Must use "gcc/6.1.0” version or higher

&#x20;

■ Compiler type

| **Compiler** | **program** | **Source extensions**                                  |
| ------------ | ----------- | ------------------------------------------------------ |
| gcc / g++    | C / C++     | .C, .cc, .cpp, .cxx,.c++                               |
| gfortran     | F77/F90     | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

■ GNU compiler option

| **Compiler option**                        | **Description**                                                                                                                                                         |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -O\[1\|2\|3]                               | Object optimization. Optimization level for numbers.                                                                                                                    |
| <p>-march=skylake-avx512<br>-march=knl</p> | <p>Supports CPU with a 512 bit register (when the SKL node is used for computing)<br>Supports MIC with a 512 bit register (when the KNL node is used for computing)</p> |
| -Ofast                                     | -O3 -ffast-math macro                                                                                                                                                   |
| -funroll-all-loops                         | Unrolls all loops                                                                                                                                                       |
| -ffast-math                                | Uses a fast floating point model                                                                                                                                        |
| -mline-all-stringops                       | Allows more inlining and improves the performance of memcpy, strlen, and memsetdp dependent codes                                                                       |
| -fopenmp                                   | Uses OpenMP-based multi-thread code                                                                                                                                     |
| -g                                         | Generates debugging information                                                                                                                                         |
| -pg                                        | Generates profiling information (gmont.out)                                                                                                                             |
| -fPIC                                      | Compiles to generate position independent code (PIC)                                                                                                                    |
| -help                                      | Outputs a list of options                                                                                                                                               |

&#x20;

■ Example of using GNU compiler

The following is an example of creating an execution file test.exe by compiling a test sample file with the GNU compiler in the **KNL computing node**.

```
$ module load craype-mic-knl gcc/7.2.0 
$ gcc –o test.exe -O3 -fPIC -march=knl test.c 
or 
$ gfortran –o test.exe -O3 -fPIC -march=knl test.f90 
$ ./test.exe
```

※ Copy the test sample file for job submission in /apps/shell/home/job\_examples

&#x20;

■ Recommended options

| **Computing node** | **Recommended options**         |
| ------------------ | ------------------------------- |
| SKL                | -O3 -fPIC -march=skylake-avx512 |
| KNL                | -O3 -fPIC -march=knl            |
| SKL & KNL          | -fPIC -mpku                     |

&#x20;****&#x20;

&#x20;****&#x20;

**③ PGI compiler**

Add a PGI compiler module version required for using the PGI compiler to be used. Available modules can be checked with the command module avail.

```
$ module load pgi/18.10
```

※ Check the available version by referring to the programming tool installation status table.

&#x20;

■ Compiler type

| **Compiler** | **program** | **Source extensions**                                  |
| ------------ | ----------- | ------------------------------------------------------ |
| pgcc / pgc++ | C / C++     | .C, .cc, .cpp, .cxx, .c++                              |
| pgfortran    | F77/F90     | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

■ PGI compiler option

| **Compiler option**                                     | **Description**                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -O\[1\|2\|3\|4]                                         | Object optimization. Optimization level for numbers.                                                                                                                                                                                                                                                                                                          |
| -Mipa=fast                                              | Optimization between procedures                                                                                                                                                                                                                                                                                                                               |
| -fast                                                   | -O2 -Munroll=c:1 -Mnoframe -Mlre –Mautoinline macro                                                                                                                                                                                                                                                                                                           |
| -fastsse                                                | Optimization supporting SSE and SSE2                                                                                                                                                                                                                                                                                                                          |
| -g, -gopt                                               | Generates debugging information                                                                                                                                                                                                                                                                                                                               |
| -mp                                                     | Uses OpenMP-based multi-thread code                                                                                                                                                                                                                                                                                                                           |
| -Minfo=mp, ipa                                          | OpenMP related information, optimization between procedures                                                                                                                                                                                                                                                                                                   |
| -pg                                                     | Generates profiling information (gmon.out)                                                                                                                                                                                                                                                                                                                    |
| <p>-Mprof=time</p><p>-Mprof=func</p><p>-Mprof=lines</p> | <p>Generates PGPROF output file</p><p>- Frequently used for generating profiling information at the time-based command level</p><p>- Generates profiling information at the function level</p><p>- Generates profiling information at the line level</p><p>(- for Mprof=lines, computing time can be significantly slow owing to an increase in overhead)</p> |
| -mcmodel medium                                         | Used when a memory allocation of 2 GB or higher is required                                                                                                                                                                                                                                                                                                   |
| <p>-tp=skylake</p><p>-tp=knl</p>                        | <p>Option exclusive for the Skylake architecture processor</p><p>Option exclusive for the KNL architecture processor</p>                                                                                                                                                                                                                                      |
| -fPIC                                                   | Compiles to generate position independent code (PIC)                                                                                                                                                                                                                                                                                                          |
| -help                                                   | Outputs a list of options                                                                                                                                                                                                                                                                                                                                     |

&#x20;

■ Example of using PGI compiler

The following is an example of creating an execution file test.exe by compiling a test sample file with the PGI compiler in the **KNL computing node**.

```
$ module load craype-mic-knl pgi/18.10 
$ pgcc –o test.exe -fast –tp=knl test.c 
or 
$ pgfortran –o test.exe -fast –tp=knl test.f90
$ ./test.exe
```

※ Copy the test sample file for job submission in /apps/shell/home/job\_examples

&#x20;

■ Recommended options

| **Computing node** | **Recommended options** |
| ------------------ | ----------------------- |
| SKL                | -fast –tp=skylake       |
| KNL                | -fast –tp=knl           |
| SKL & KNL          | -fast –tp=skylake,knl   |

&#x20;

&#x20;

**④ Cray compiler**

Add a Cray compiler module version required for using the Cray compiler to be used. Available modules can be checked with the command module avail.

```
$ module load cce/8.6.3
```

※ Check the available version by referring to the programming tool installation status table.

&#x20;

■ Compiler type

| **Compiler** | **program** | **Source extensions**                                  |
| ------------ | ----------- | ------------------------------------------------------ |
| cc / CC      | C / C++     | .C, .cc, .cpp, .cxx,.c++                               |
| ftn          | F77/F90     | .f, .for, .ftn, .f90, .fpp, .F, .FOR, .FTN, .FPP, .F90 |

&#x20;

■ Compiler option

| **Compiler option**                             | **Description**                                                              |
| ----------------------------------------------- | ---------------------------------------------------------------------------- |
| -O\[1\|2\|3]                                    | Object optimization. Optimization level for numbers.                         |
| -hcpu=mic-knl                                   | Supports MIC with a 512 bit register                                         |
| <p>-Oipa[0|1|2|3|4|5]<br>-hpia[0|1|2|3|4|5]</p> | <p>-ftn Inlining option<br>cc/CC Inlining option</p>                         |
| -hunroll\[0\|1\|2]                              | Unrolling option. Unrolls all loops if Default is 2                          |
| -hfp\[0\|1\|2\|3\|4]                            | Floating\_Point optimization                                                 |
| -homp(default)                                  | Uses OpenMP-based multi-thread code                                          |
| -g \| -G0                                       | Generates debugging information                                              |
| -h pic                                          | Used when a static memory of 2 GB or higher is required (used with -dynamic) |
| -dynamic                                        | Links shared libraries                                                       |

&#x20;

■ Example of using Cray compiler

The following is an example of creating an execution file test.exe by compiling a test sample file with the PGI compiler in the **KNL computing node**.

```
$ module load craype-mic-knl cce/8.6.3 PrgEnv-cray/1.0.2 
$ cc –o test.exe –hcpu=mic-knl test.c 
or 
$ ftn –o test.exe –hcpu=mic-knl test.f90 
$ ./test.exe
```

※ Copy the test sample file for job submission in /apps/shell/home/job\_examples

&#x20;

■ Recommended options

| **Computing node** | **Recommended options** |
| ------------------ | ----------------------- |
| SKL                | Default value           |
| KNL                | -hcpu=mic-knl           |
| SKL & KNL          | Default value           |

※ test.c and test.f90 for testing can be found in **/apps/shell/home/job\_examples** (test by copying to the user directory)

※ For programs to use the KNL optimization option, it is recommended to access them through interactive job submission by the KNL debug node and then compile (refer to “Job execution through scheduler → B. Job submission monitoring → 2) Interactive job submission”).

&#x20;

3\) Compiling parallel programs

(1) OpenMP compile

OpenMP is a technique simply developed to enable multi-thread utilization only by a compiler directive. A compiler used for compiling parallel programs using OpenMP is the same as that of sequential programs. A compiler option can be added for parallel compilation, and most compilers currently support the OpenMP directive.

| **Compiler option**      | **Program**       | **Option** |
| ------------------------ | ----------------- | ---------- |
| icc / icpc / ifort       | C / C++ / F77/F90 | -qopenmp   |
| gcc / g++ / gfortran     | C / C++ / F77/F90 | -fopenmp   |
| cc / CC /ftn             | C / C++ / F77/F90 | -homp      |
| pgcc / pgc++ / pgfortran | C / C++ / F77/F90 | -mp        |

&#x20;

① Example of OpenMP program compilation (Intel compiler)

The following is an example of creating an execution file test\_omp.exe by compiling a test\_omp sample file that uses OpenMP with the Intel compiler in the **KNL computing node**.

```
$ module load craype-mic-knlintel/18.0.3 
$ icc -o test_omp.exe -qopenmp -O3 -fPIC –xMIC-AVX512 test_omp.c 
or 
$ ifort -o test_omp.exe -qopenmp -O3 -fPIC –xMIC-AVX512 test_omp.f90 
$ ./test_omp.exe
```

&#x20;

② Example of OpenMP program compilation (GNU compiler)

The following is an example of creating an execution file test\_omp.exe by compiling a test\_omp sample file that uses OpenMP with the GNU compiler in the **KNL computing node**.

```
$ module load craype-mic-knl gcc/7.2.0 
$ gcc -o test_omp.exe -fopenmp -O3 -fPIC -march=knl test_omp.c 
or $ gfortran -o test_omp.exe -fopenmp -O3 -fPIC -march=knl test_omp.f90 
$ ./test_omp.exe
```

&#x20;

③ Example of OpenMP program compilation (PGI compiler)

The following is an example of creating an execution file test\_omp.exe by compiling a test\_omp sample file that uses OpenMP with the PGI compiler in the **KNL computing node**.

```
$ module load craype-mic-knl pgi/18.10 
$ pgcc –o test_omp.exe -mp -fast test_omp.c 
or 
$ pgfortran –o test_omp.exe -mp -fast test_omp.f90 
$ ./test_omp.exe
```

&#x20;

④ Example of OpenMP program compilation (Cray compiler)

The following is an example of creating an execution file test\_omp.exe by compiling a test\_omp sample file that uses OpenMP with the Cray compiler in the **KNL computing node**.

```
$ module load craype-mic-knl cce/8.6.3 PrgEnv-cray/1.0.2 
$ cc -o test_omp.exe -homp -hcpu=mic-knl test_omp.c 
or 
$ ftn -o test_omp.exe -homp -hcpu=mic-knl test_omp.f90 
$ ./test_omp.exe
```

&#x20;

(2) MPI compiler

Users can execute the MPI commands in the following table, and these commands are a type of wrapper where a designated compiler compiles the source through .bashrc.

| **Category**  | **Intel** | **GNU**  | **PGI**   | **Cray** |
| ------------- | --------- | -------- | --------- | -------- |
| Fortran       | ifort     | gfortran | pgfortran | ftn      |
| Fortran + MPI | mpiifort  | mpif90   | mpif90    | ftn      |
| C             | icc       | gcc      | pgcc      | cc       |
| C + MPI       | mpiicc    | mpicc    | mpicc     | cc       |
| C++           | icpc      | g++      | pgc++     | CC       |
| C++ + MPI     | mpiicpc   | mpicxx   | mpicxx    | CC       |

Even when compiled through mpicc, the options corresponding to the original compiler being wrapped must be used.

&#x20;****&#x20;

**①** Example of MPI program compilation (Intel compiler)

The following is an example of creating an execution file test\_mpi.exe by compiling a test\_mpi sample file that uses MPI with the Intel compiler in the **KNL computing node**.

```
$ module load craype-mic-knl intel/18.0.3 impi/18.0.3 
$ mpiicc -o test_mpi.exe -O3 -fPIC -xMIC-AVX512 test_mpi.c 
or 
$ mpiifort -o test_mpi.exe -O3 -fPIC -xMIC-AVX512 test_mpi.f90 
$ mpirun -np 2 ./test_mpi.exe
```

&#x20;

**②** Example of MPI program compilation (GNU compiler)

The following is an example of creating an execution file test\_mpi.exe by compiling a test\_mpi sample file that uses MPI with the GNU compiler in the **KNL computing node**.

```
$ module load craype-mic-knl gcc/7.2.0 openmpi/3.1.0 
$ mpicc -o test_mpi.exe -O3 -fPIC -march=knl test_mpi.c 
or 
$ mpif90 -o test_mpi.exe -O3 -fPIC -march=knl test_mpi.f90 
$ mpirun -np 2 ./test_mpi.exe
```

&#x20;

**③** Example of MPI program compilation (PGI compiler)

The following is an example of creating an execution file test\_mpi.exe by compiling a test\_mpi sample file that uses MPI with the PGI compiler in the **KNL computing node**.

```
$ module load craype-mic-knl pgi/18.10 openmpi/3.1.0 
$ mpicc -o test_mpi.exe -O3 -fPIC -tp=knl test_mpi.c 
or 
$ mpifort -o test_mpi.exe -O3 -fPIC -tp=knl test_mpi.f90 
$ mpirun -np 2 ./test_mpi.exe
```

&#x20;

**④** Example of MPI program compilation (Cray compiler)

The following is an example of creating an execution file test\_mpi.exe by compiling a test\_mpi sample file that uses MPI with the Cray compiler in the **KNL computing node**.

```
$ module load craype-mic-knl cce/8.6.3 PrgEnv-cray/1.0.2 
$ cc -o test_mpi.exe -hcpu=mic-knl test_mpi.c 
or 
$ ftn -o test_mpi.exe -hcpu=mic-knl test_mpi.f90 
$ mpirun -np 2 ./test_mpi.exe
```

&#x20;
