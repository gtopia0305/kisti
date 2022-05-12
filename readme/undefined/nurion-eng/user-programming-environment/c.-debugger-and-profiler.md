---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > User Programming Environment > C. Debugger
  and Profiler
---

# C. Debugger and Profiler

The 5th supercomputer Nurion beta service provides DDT for program debugging by users. Furthermore, two profilers, namely Intel vtune and CaryPat, are provided for the program profiling of users.

1\) Example of using debugger DDT\
\- Select the architecture, compiler, and MPI to be used for using DDT in the 5th supercomputer, and then select a module for using DDT.

```
$ module load craype-mic-knl or craype-x86-skylake
$ module load intel/17.0.5 impi/17.0.5
$ module load forge/18.1.2
```

\- This example was tested in the identical environment as above.

\- Select an execution file by adding the -g -O0 option when compiling as preparation before using DDT.

```
$ mpiicc -o test.x -g -O0 test.c
```

\- After running xming and completing the settings of the SSH X environment on a user’s desktop, execute the DDT execution command.

```
$ ddt &
```

\- Execute the command to see if the following pop-up execution window appears.

![](<../../../../.gitbook/assets/명령을 실행하여 다음과 같은.png>)



* Select “RUN” among the listed commands, select the file for debugging as shown below, and then click “RUN” in the new pop-up window.

![](<../../../../.gitbook/assets/위 명령중 RUN.png>)



* Debugging can be initiated by entering the debugging mode for the following selected execution file.

![](<../../../../.gitbook/assets/다음과 같이 선택 된.png>)

2\) Example of using profiler Intel vtune Amplifier\
Select the architecture, compiler, and MPI for using a profiler vtune in this system, and then select vtune to use the profiler.

```
$ module load craype-mic-knl or craype-x86-skylake 
$ module load intel/17.0.5 impi/17.0.5
$ module load vtune/17.0.5
```

This example was tested in an identical environment as above.

ㅇ How to use CLI\
\- The command for executing the Intel vtune Amplifier in CLI mode is as follows.

```
$ Executing a program for analyzing the amplxe-cl option
```

```
$ amplxe-cl -collect hotspots /home01/$USER/test/test.x  

amplxe: Using result path '/home01/$USER/test/r000hs' 
amplxe: Executing actions 75% Generating a report 
Function CPUTime CPUTime:EffectiveTime CPUTime:EffectiveTime:Idle 
CPUTime:EffectiveTime:Poor CPUTime:EffectiveTime:Ok CPUTime:EffectiveTime:Ideal 
CPUTime:EffectiveTime:Over CPUTime:SpinTime CPUTime:OverheadTime Module Function(full) 
SourceFile StartAddress   

multiply1 21.568s 0.176s 21.392s 0s 0s 0s 0s martix.mic multiply1 multiply.c 
0x401590 
init_arr 0.020s 0s 0.020s 0s 0s 0s 0s matrix.mic init_arr matrix.c
0x402384 
amplx: Executing actions 100% done
```

\- If a compiled execution file is prepared and executed according to the command, the r000hs directory is generated. After confirming that the directory has been generated, the command for generating a report is executed to output the result shown below.

```
$ amplxe-cl -report hotspots /home01/$USER/debugger/test/r000hs 
amplxe: Using result path `/home01/$USER/debugger/test/r000hs' 
amplxe: Executing actions 75 % Generating a report Function CPU Time CPU Time:Effective Time CPU Time:Spin Time CPU Time:Overhead Time Module Function (Full) Source File Start Address -------- -------- ----------------------- ------------------ ---------------------- ------ --------------- ----------- ------------- 
amplxe: Executing actions 100 % done
```

ㅇ How to check the result of using GUI\
\- Intel vtune Amplifier also supports the GUI mode. Only the method for checking the result using GUI is explained here.

\- Run xming on a user’s desktop

```
$ amplxe-gui
```

\- Click “New Analysis” in the screen below.

![](<../../../../.gitbook/assets/아래의 화면에서 New.png>)



* When the screen shown below appears, check the number of CPUs and click the Start button to begin the analysis.

![](<../../../../.gitbook/assets/아래와 같은 화면이 나타나면 cpu.png>)



* Once completed, the analysis results are summarized in multiple tabs as shown below.

![](<../../../../.gitbook/assets/완료되면 다음과 같이 분석 결과가.png>)

![](<../../../../.gitbook/assets/완료되면 다음과 같이 분석 결과가(1).png>)

3\) Example of using profiler Cary-Pat\
The environment including the architecture is set as below for using CaryPat, which is a profiler, and the example was initiated.

```
$ module load craype-mic-knl or craype-x86-skylake 
$ module load perftools-base/6.5.2 perftools/6.5.2 
$ module load PrgEnv-cray/1.0.2 cce/8.6.3
```

\- First, test.c file to be used in the example is compiled.

```
$ cc test.c
```

\- As a result, an execution file a.out is generated.

\- For analyzing with CaryPat, use pat\_build to generate a new execution file.

```
$ pat_build -0 apa a.out
```

\- As a result, a.out+pat file is generated.

\- The generated execution file is written with MPI; thus, it is executed with mpirun.

```
$ mpirun -np 4 ./a.out+pat
```

\- Once execution is complete, the a.out+pat+378250-3s directory is generated, and the xf-files/002812.xf file is created in the directory.

```
$ pat_report a.out+pat+378250-3s
```

\- When pat\_report is executed as above, .ap2 and .apa files are created in the a.out+pat+378250-3s directory.

```
$ pat_build -0 a.out+pat+378250-3s/build-options.apa
```

\- When the execution file is created again using the .apa file, the file named a.out+apa is created.

\- When the generated a.out+apa file is executed

```
$ mpirun -np 4 ./a.out+apa
```

\- a new xf file is generated in a.out+pat+378250-3t.

\- Reuse pat\_report to process the new data.

```
$ pat_report a.out+apa+378250-3t
```

\- When the file is executed as above, the ap2 file and tracing report are created.

&#x20;

\- app2 is provided as a method for visualizing the collected data.

```
$ app2 a.out+apa+378250-3t
```

&#x20;

\- Visualization results are produced as shown below.

![](<../../../../.gitbook/assets/아래와 같이 시각화가 가능하다..png>)

![](<../../../../.gitbook/assets/아래와 같이 시각화가 가능하다.(1).png>)

![](<../../../../.gitbook/assets/아래와 같이 시각화가 가능하다.(2).png>)

![](<../../../../.gitbook/assets/아래와 같이 시각화가 가능하다.(3).png>)
