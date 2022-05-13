---
description: '[Appendix 1] Main Keywords for Job Scripts'
---

# \[Appendix 1] Main Keywords for Job Scripts

You need to specify how to allocate resources for the desired job by using appropriate keywords in the job script. The main keywords are shown below, and users can create a job script file using only a few of these keywords.

&#x20;

| **Option**      | **Format**                                  | **Description**                                         |
| --------------- | ------------------------------------------- | ------------------------------------------------------- |
| -A              |                                             | Specify an accounting string                            |
| -a              | \[\[\[\[YYYY]MM]DD]hhmm\[.S]]               | Scheduled execution                                     |
| -C              | ""                                          | Change the prefix of the PBS directive (ex>#PBS)        |
| -c              | \[c \| c=numeric \| w \| w= \| n \| s \| u] | Specify the checkpoint cycle for the job                |
| -e              |                                             | Specify the path of the error file                      |
| -h              |                                             | Delay job execution (delayed execution)                 |
| -I \| -X        |                                             | X11 forwarding activated interactive job                |
| -J              | ]                                           | Define a job array                                      |
| -j              | \[o \| e]                                   | Merge the output and error files                        |
| -k              | \[o \| e]                                   | Keep the output and error files on the execution host   |
| -l              |                                             | Request job resources                                   |
| -M              |                                             | Set the list of email recipients                        |
| -m              |                                             | Specify the email notification                          |
| -N              |                                             | Specify the job name                                    |
| -o              |                                             | Specify the path of the output file                     |
| -P              |                                             | Specify the project name                                |
| -p              |                                             | Set the priority of the job                             |
| -q              |                                             | Specify the name of the server or queue                 |
| -r              | \[Y \| N]                                   | Mark whether the job can be rerun                       |
| -S              |                                             | Specify the script language to be used                  |
| -u              |                                             | Specify the user ID of the job                          |
| -V              |                                             | Export environment variables                            |
| -v              |                                             | Expand the environment variables                        |
| -W              |                                             | Set the attribute values of the job                     |
| -W block=       |                                             | Request qsub to wait until the job is finished          |
| -W depend=      |                                             | Specify job dependencies                                |
| -W group\_list= |                                             | Specify the group ID of the job owner                   |
| -W pwd=         | ""                                          | Password method per job                                 |
| -W run\_count=  |                                             | Regulate the number of times the job is run             |
| -W sandbox=     | \[HOME \| PRIVATE]                          | Staging directory and execution directory               |
| -W stagein=     |                                             | Input file staging                                      |
| -W stageout=    |                                             | Output file staging                                     |
| -W unmask=      |                                             | Specify the UNIX job umask                              |
| -X              |                                             | X output from interactive jobs                          |
| -z              |                                             | Suppress job identifier                                 |

※ Manual with details: Refer to [https://pbsworks.com/pdfs/PBSUserGuide14.2.pdf](https://pbsworks.com/pdfs/PBSUserGuide14.2.pdf)&#x20;
