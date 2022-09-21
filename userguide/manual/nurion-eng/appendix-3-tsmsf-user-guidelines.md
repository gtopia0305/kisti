# \[Appendix 3] TSMSF User Guidelines

**Method for Archiving Data Using TSMSF**

## A. System Environment and How to Use the System

![](<../../../.gitbook/assets/\[별첨5] TSMSF를 이용한 데이터 아카이빙\_1.png>) ![](<../../../.gitbook/assets/\[별첨5] TSMSF를 이용한 데이터 아카이빙.png>)

The Nurion system operates a Tape Library (IBM TS4500) with a capacity of 10 PBytes to support the long-term (1 year after the contract expires) storage of users' important files and large files.

Tiered storage management script facility (TSMSF) is a software tool that supports the backup and archiving of users' data, and it is installed and operated on the Datamover node of the Nurion system. TSMSF supports the automatic archiving of user data and the manual restoration of data when the user needs the data. Unlike the home directory (/home01) backup, which is performed regularly by the administrator, the user has to upload files to a specific directory (/scratch/arcv/\[$USER]). As shown in the figure, TSMSF currently consists of a tape library and Datamover (DM) nodes.

**o Nurion system TSMSF: Tape (10PB)**

**o Nurion system Datamover Node: nurion-dm.ksc.re.kr**

![\[TSMSF System Configuration Diagram \]](../../../.gitbook/assets/K2uTYs6pSXjH8F6.bmp)

Data are automatically archived 7 days (this may be changed later) after the user uploads files from Datamover to the specified directory (/scratch/arcv/$USER). When uploading files to Datamover, the user can use FTP, SCP, SSH, SFTP, etc. to upload files directly from the outside. The user can also upload files through the Nurion login node (but FTP cannot be used for the login node). Here, services other than FTP require the user to enter a one-time password (OTP). Furthermore, services other than FTP automatically stop uploading files if the limited resource (10 min of CPU time) is used up. Hence, we recommend that you use FTP to upload files.

## B. User Policy and Application Method

The amount of data that can be stored in TSMSF per account (ID) is 100 TB (a maximum of 1,000,000 files). The data are retained up to 1 year after the user account expires.

Because TSMSF uses tape media, **it may be difficult to recover the data in the event of a tape media failure.** Therefore, **it is better to keep important data on the user's local system.**

In addition, it is generally time consuming to load tape media into the tape library for access. Therefore, if there are many small files to backup or archive, such as source files, it is better to archive the files after compressing them using tar or gzip.

**To use TSMSF, a separate user environment needs to be configured. Therefore, please send an email to account@ksc.re.kr to request authorization to use TSMSF. (A separate form is not required, and you only need to send your account information (ID).)**

For other inquiries related to general technical support, please send an email to consult@ksc.re.kr.

## C. Archiving Method

**3.1 Connecting to a login node and a Datamover node**

To archive the user data, please use the Nurion system's login node or Datamover node. When you first log in, the directory location is “/home01/$USER.” Note that this directory is the general home directory of the Nurion system, and not the home directory of TSMSF. Therefore, change the path using the cd command (cd /scratch/arcv/$USER) to move to the user directory for TSMSF.

(Method1) Connect (ssh) to the login node (nurion.ksc.re.kr) or the Datamover node (nurion-dm.ksc.re.kr), and use the cp command.

```
[user01@dm2:/home01/user01 ]$ ssh $USER@nurion-dm.ksc.re.kr (또는 ssh $USER@nurion.ksc.re.kr)
  Password(OTP):
  Password:

[user01@dm02:/home01/user01 ]$ cp –r [mydir | myfile] /scratch/arcv/$USER/
```

(Method 2) Use the scp command remotely.

```
[user01@login01:/home01/user01 ]$ scp –r [mydir | myfile] $USER@nurion-dm.ksc.re.kr:/scratch/arcv/$USER/
  Password(OTP):
  Password:
```

(Method 3) Upload files by connecting to the Datamover node (nurion-dm.ksc.re.kr) using an FTP / SFTP client program.



① Example of using the FileZilla client (FTP)

\- Enter the information to connect to the Datamover node, and click the “Quick connect” button.

| Host                | User name              | Password      | Port |
| ------------------- | ---------------------- | ------------- | ---- |
| nurion-dm.ksc.re.kr | USER ID (user account) | User password | 21   |

![](../../../.gitbook/assets/09vnGgvqndHwOJR.bmp)

\- Because the path of the remote site is the user home directory (/home01/$USER), move to the specified directory (/scratch/arcv/$USER).

※ You can move easily by directly entering the absolute path in the remote site path.

![](../../../.gitbook/assets/lIkNBrVqqyiDIXz.bmp)

\- Upload files by selecting files or directories in the local site on the left.

![](../../../.gitbook/assets/zOsAQCEcNmolm8w.bmp)

※ Refer to (https://filezilla-project.org/) for a more detailed FileZilla manual.

② Example of using the SSH Secure Shell Client (SFTP)

\- Click the “Quick Connect” button to connect to the Datamover node.

![](../../../.gitbook/assets/tMoayQXkQ5bJOKX.png)

\- Enter the information to connect to the Datamover node, and click the “Connect” button.

| Host                | User name              | Port | Authentication method |
| ------------------- | ---------------------- | ---- | --------------------- |
| nurion-dm.ksc.re.kr | USER ID (user account) | 22   | Keyboard Interactive  |

![](../../../.gitbook/assets/HvjeKpQhjBDTD80.bmp)

\- Enter the one-time password (OTP) and the password sequentially, and then click the “OK” button.

![](<../../../.gitbook/assets/tvuPaZyizUkrei0 (1).bmp>)

![](<../../../.gitbook/assets/zZBM6Z3RBI0iRJ8 (1).bmp>)

\- Because the path of the site on the right is the user home directory (/home01/$USER), move to the specified directory (/scratch/arcv/$USER).

※ You can move easily by directly entering the absolute path in the site path.

![](../../../.gitbook/assets/RTVTku7zIhHsrPv.bmp)

\- Upload files by selecting files or directories in the local site on the left.

&#x20;

![](../../../.gitbook/assets/wlvqaTAw4xYiieb.bmp)

**3.2 Data archiving**

Among the files in the specified directory (/scratch/arcv/$USER), the files that are larger than 10 Mbytes and have not been accessed for 3 days (to be changed later) are automatically archived. For the archived files, only the temporary files (chunk files) with a file size of 0 bytes remain in that directory, as shown below, and the actual data exists in the tape library. If the number of files increases, it takes longer to archive the files to the tape library or restore the files to the disk for use. Hence, if possible, it is recommended that the number of files be reduced by compressing (e.g., tar) them.

| <p>[user01@dm2 ~] <strong>ls –lh /scratch/arcv/$USER/*</strong></p><p>... omitted ...</p><p>-rw-r--r-- 1 root root 0 Feb 5 18:51 2019-2-19-18-51.[File Name].archived</p><p>... omitted ...</p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

o Example

| <p># ls -l <strong>/scratch/arcv/testdir/</strong></p><p>total 2744320</p><p>-rw-r--r-- 1 root root 0 Feb 25 20:48 2019-2-25-20-48.test.220M.archived</p><p>-rw-r--r-- 1 root root 0 Feb 25 20:48 2019-2-25-20-48.test.230M.archived</p><p>-rw-r--r-- 1 root root 104857600 Sep 27 14:54 test.100M</p><p>-rw-r--r-- 1 root root 10485760 Sep 27 14:54 test.10M</p><p>-rw-r--r-- 1 root root 115343360 Sep 27 14:54 test.110M</p><p>... omitted ...</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**3.3 Checking the detailed information about the archived files**

For the archived data, the arc\_ls command can be used to check the detailed information about the files, as shown below.

| <p># arc_ls <strong>/scratch/arcv/testdir/</strong></p><p>DIR: /scratch/arcv/testdir</p><p>ARC: /scratch/arcv/testdir/2019-2-25-20-48.test.460M.archived</p><p>- File to restore : /scratch/arcv/testdir/test.460M</p><p>- File owner : 0</p><p>- File group : 0</p><p>- File size : 482344960</p><p>- Archived Name : 3_7_4_6_scratcharcvtestdirtest.460M</p><p>- Archived time : 1551095210</p><p>NRM: /scratch/arcv/testdir/test.10M</p><p>... omitted ...</p> |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

The arc\_ls command can take the relative or absolute path of a directory or file as an argument. It can also search the subdirectories of the specified directory through the -r option when needed. In the output result, DIR denotes a directory, NRM a regular file, and ARC an archived file.



## D. Restore Method

To use the archived data, the files need to be restored. The command used to restore files is arc\_restore. **This command is time consuming to process because the data files are retrieved from the tape library.** Archived files can be restored as follows:

| <p>[user01@dm2 ~] <strong>arc_restore /scratch/arcv/$USER/[*.archived]</strong></p><p>... omitted ...</p><p>-rw-r--r-- 1 root root 200M Feb 5 18:51 <strong>[original file]</strong></p><p>... omitted ...</p> |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

o Example

| <p># arc_restore arc_restore /scratch/arcv/testdir/2019-2-25-20-49.test.580M.archived</p><p>ARCRESTORE : start to find archived files.</p><p>Processing restore queue...</p><p>[207249:207265] resRunner : filecheck : ADD : 1/0/0 : /scratch/arcv/testdir/test.580M</p><p>[207265:207269] Process ID 207269 started for restoring...</p><p>[207265:207269] File to restore : /scratch/arcv/testdir/test.580M</p><p>[207265:207269] File owner : 0</p><p>[207265:207269] File group : 0</p><p>[207265:207269] File size : 608174080</p><p>[207265:207269] Archived Name : 3_7_4_6_scratcharcvtestdirtest.580M</p><p>[207265:207269] Archived time : 1551095339</p><p>[207265:207269] Restore started at : Thu Feb 28 12:16:43 2019</p><p>[207265:207269] restoreFile : sh PTLGet -p 3_7_4_8_scratcharcvtestdirtest.580M -d /scratch/arcv/testdir -t 1551095339</p><p>[207265:207269] restoreFile : move /scratch/arcv/testdir/.working/3_7_4_6_scratcharcvtestdirtest.580M to /scratch/arcv/testdir/test.580M</p><p>[207265:207269] restoreFile : unlink /scratch/arcv/testdir/2019-2-25-20-49.test.580M.archived</p><p>[207265:207269] restoreFile : dbupdate( ARCHIVED_FILES / last_mod=1551323868,restore_count=1,status_id=1,result_id=1 Where file_id=341 )</p><p>[207265:207269] Restoring was done : /scratch/arcv/testdir/test.580M</p><p>[207249:207265] resRunner : CProc was forked : CProc count=1, CProc tCount=1</p><p>[207249:207265] resRunner : [ success=1 / fail=0 ] 207269 was finished : Success</p><p>resRunner : REPORT : resresult : Success=1 / Fail=0</p><p>Main thread runtime = 00:01:05</p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

Similar to the arc\_ls command, the arc\_restore command can take the relative or absolute path of a directory or file as an argument. It can also search the subdirectories of the specified directory through the -r option when needed. In addition, either the chunk file name (timestamp.\[File Name].archived) or the original file name can be specified when specifying the file.

## E. Delete Method

To delete some or all of the files in the specified directory (/scratch/arcv/$USER), you can use the rm command, which is a basic Linux command.

Temporary files (chunk files with the .archived extension) can also be deleted using the rm command. When temporary files are deleted, it may take about 3 days for the archived data in the tape library to be deleted and synchronized. That is, if the temporary file does not exist in the original path, the system determines that "the user has deleted the file." Therefore, do not change the name of the temporary file directly or move the temporary file. You must first restore the file before changing the temporary file name or moving the temporary file. **(Please note that it is impossible to restore the file once the temporary file has been deleted.)**

{% hint style="info" %}
2022년 2월 15일에 마지막으로 업데이트되었습니다.
{% endhint %}
