# \[Appendix 4] Method for Using the MVAPICH2 Performance Optimization Option

You can use the environment variables within the job script to set the inter-process communication algorithm when using MVAPICH2 as an MPI library. The user can optimize the execution performance by setting the environment variables for the collective communication, which is often used in the code. The main keywords are as follows.

The <mark style="color:red;">**MV2\_INTRA**</mark> environment variable is used for the intra-node MPI process communication, and the <mark style="color:blue;">**MV2\_INT**</mark>**ER** environment variable is used for the inter-node MPI process communication. For MPI\_Alltoall, the same algorithm is applied to both the INTRA and INTER cases. The same can be applied to routines for transmitting data of different sizes (example: MPI\_Alltoallv) or non-blocking-based routines (example: MPI\_Ibcast), and the setting method is shown below.

&#x20;

■ Main environment variables for the MVAPICH2 library collective communication

| MPI communication |                                                                          Environment variable                                                                         | Setting range |
| :---------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----------: |
|    MPI\_Gather    |    <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_GATHER_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_GATHER_TUNING</p>    |     0 \~ 3    |
|     MPI\_Bcast    |     <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_BCAST_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_BCAST_TUNING</p>     |    0 \~ 10    |
|    MPI\_Scatter   |   <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_SCATTER_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_SCATTER_TUNING</p>   |     1 \~ 5    |
|   MPI\_Allreduce  | <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_ALLREDUCE_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_ALLREDUCE_TUNING</p> |     1 \~ 6    |
|    MPI\_Reduce    |    <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_REDUCE_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_REDUCE_TUNING</p>    |     1 \~ 6    |
|   MPI\_Allgather  | <p><mark style="color:red;"><strong>MV2_INTRA</strong></mark>_ALLGATHER_TUNING</p><p><mark style="color:blue;"><strong>MV2_INTER</strong></mark>_ALLGATHER_TUNING</p> |    1 \~ 11    |
|   MPI\_Alltoall   |                                                                         MV2\_ALLTOALL\_TUNING                                                                         |     0 \~ 4    |

※ Usage example (when creating a bash script): export MV2\_INTER\_ALLREDUCE\_TUNING=2

※ Refer to [http://mvapich.cse.ohio-state.edu/userguide/](http://mvapich.cse.ohio-state.edu/userguide/) for a detailed description of each environment variable.

&#x20;
