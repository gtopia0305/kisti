# 코드블록 이슈

코드블럭 이슈 - 내부 텍스트 색상변환 안

```
#!/bin/sh#PBS -V
#PBS -N <mark style="color:blue;">Nastran_job</mark>
#PBS -q comrcial
#PBS -l select=1i:ncpus=40:mpiprocs=1:ompthreads=40#PBS -l walltime=04:00:00#PBS -A nastrancd $PBS_O_WORKDIR/apps/commercial/MSC/Nastran/bin/nast20182 car_mod_freq.bdf smp=$NCPUS batch=no sdir="."
```

<mark style="color:red;"></mark>

<mark style="color:red;">텍스트 색상 변환 레드</mark>

텍스트 색상 변환 블루



