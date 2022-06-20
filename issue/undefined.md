# 코드블록 이슈

코드블럭 이슈 - 내부 텍스트 색상변환 안됨  

```
#PBS -V
#PBS -N <mark style="color:#0000ff">Nastran_job</mark>
#PBS -q commercial
#PBS -l select=1:ncpus=<span style="color:#0000ff">40</span>:mpiprocs=1:ompthreads=<span style="color:#0000ff">40</span>
#PBS -l walltime=<span style="color:#0000ff">04:00:00</span>
```

<pre class="highlight">#!/bin/sh
#PBS -V
#PBS -N <mark style="color:#0000ff">Nastran_job</mark>
#PBS -q commercial
#PBS -l select=1:ncpus=<span style="color:#0000ff">40</span>:mpiprocs=1:ompthreads=<span style="color:#0000ff">40</span>
#PBS -l walltime=<span style="color:#0000ff">04:00:00</span>
<span style="color: #ff0000;">#PBS -A nastran</span>

cd $PBS_O_WORKDIR

/apps/commercial/MSC/Nastran/bin/nast20182 <span style="color: #ff0000;">car_mod_freq.bdf</span> smp=$NCPUS batch=no sdir="."
</pre>



<mark style="color:red;"></mark>

<mark style="color:red;">텍스트 색상 변환 레드</mark>
```
<mark style="color:red;">텍스트 색상 변환 레드</mark>
```


<span style="color:red;">텍스트 색상 변환 레드</span>
```
<span style="color:red;">텍스트 색상 변환 레드</span>
```
