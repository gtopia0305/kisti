# User Environment

## A. Account Creation

① Researchers that have been approved to use the Nurion system should access the KISTI website ([**https://www.ksc.re.kr**](https://www.ksc.re.kr/))  to apply for an account.

◦ How to apply : Access the KISTI website, (top) Application for use -> (top) Apply -> Select application

\- Free account : Nurion system innovative support program, new users of the 5th supercomputer

\- Paid account : General and student users of the 5th supercomputer

\- Once an account has been created, information related to the account will be sent to the email listed on the application.

&#x20;

② Request for OTP (One Time Password) authentication code

**Based on the account information email you have received,** send an email to [account@ksc.re.kr](mailto:account@ksc.re.kr) by referring to the example below to receive an authentication code.

&#x20;

| Subject              | <p>Request for OTP authentication code - User ID</p><p>(e.g.) Request for OTP authentication code - x123abc</p>      |
| -------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Recipient            | [account@ksc.re.kr](mailto:account@ksc.re.kr)                                                                        |
| Email body (example) | <p>Login ID: x123abc</p><p>Mobile number: 010-1234-5678</p><p>Name: John Doe</p><p>Carrier: LG Uplus (or SKT/KT)</p> |

&#x20;

③ Installing OTP application

\- The OTP smartphone application is provided for secured access to the supercomputer.

\- For the OTP smartphone application, search for “Any OTP” on Google Play or App Store and install the application developed by Mirae-tech.

\- When logging into the supercomputer, the OTP security code of the “Any OTP” application must be entered.

&#x20;

※ If you do not use a smartphone, please consult the account manager (account@ksc.re.kr).

※ Refer to the “OTP User Manual” by accessing the KISTI website > Technical Support > Guidelines for more details on OTP installation and use

※ Emails will be sent out for users on the LG U+ network because text messages are processed as spam on this network

&#x20;

## B. Login

\- Users can gain access through the login node (nurion.ksc.re.kr) of the Nurion system (refer to the node configuration below).

\- Default character set (encoding) is in unicode (UTF-8).

\- Only SSH, SCP, SFTP, and X11 are allowed to access the login node.

&#x20;

① Unix or Linux environment

```
$ ssh -l nurion.ksc.re.kr [-p 22]
```

e.g.) When user ID is x123abc

```
$ ssh -l x123abc nurion.ksc.re.kr or $ ssh -l x123abc nurion.ksc.re.kr -p 22
```

&#x20;

② Windows environment

\- Run Xming to execute the X environment

&#x20;※ Download the program for free from the internet and install it.

![](../../../.gitbook/assets/JJNmXayKPdXGask.png)

&#x20;

\- Use ssh access programs such as putty or SSH Secure Shell Client

&#x20;※ Programs can be downloaded for free from the internet.

&#x20;※ Host Name : nurion.ksc.re.kr, Port : 22, Connection type : SSH

![](../../../.gitbook/assets/mymgSTp1SpIXNqv.png)

&#x20;

&#x20;※ ssh -> X11 tap -> check "Enable X11 forwarding"

&#x20;※ X display location : localhost:0.0

![](../../../.gitbook/assets/7CIgfnisxwUP8q8.png)

&#x20;

&#x20;※ If the system cannot be accessed because of a DNS caching problem, clear all cache (run ipconfig/flushdns in command prompt) and access again.

```
C: ipconfig /flushdns
```

&#x20;

③ Sending/receiving files

&#x20;\- Access the Datamover node (nurion-dm.ksc.re.kr) through an FTP client to send/receive files (refer to the node configuration below).

&#x20;

```
$ ftp nurion-dm.ksc.re.kr or $ sftp [user ID@]nurion-dm.ksc.re.kr [-p 22]
```

&#x20;\- In the Windows environment, use FTP/SFTP client programs such as WinSCP that are distributed for free to access the system.

![](../../../.gitbook/assets/cmp2uAQLNNDJaOJ.png)

&#x20;\* File Transfer Protocol (FTP) is used, where files can be sent without entering the OTP

&#x20;\* Secure-FTP (SFTP) is used, where the OTP must be entered when sending files (safer than FTP).

&#x20;

④ Node configuration

&#x20;

|  ****          | **Host name**       | **CPU Limit**    | **Note**                                                                                            |                                                                                  |
| -------------- | ------------------- | ---------------- | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Login node     | nurion.ksc.re.kr    | 20 min.          | <p>ㆍSSH/SCP accessible<br>ㆍFor compile and batch job submission<br>ㆍFTP/SFTP access not allowed</p> |                                                                                  |
| Datamover node | nurion-dm.ksc.re.kr | -                | <p>ㆍSSH/SCP/SFTP accessible<br>ㆍFTP accessible<br>ㆍCompile and job submission not allowed</p>       |                                                                                  |
| Computing node | KNL                 | node\[0001-8305] | -                                                                                                   | <p>ㆍJob executable through PBS scheduler<br>ㆍGeneral user access not allowed</p> |
| CPU-only       | CPU\[0001-0132]     | -                |                                                                                                     |                                                                                  |

&#x20;

## C. Changing User Shell

&#x20;\- BASH is provided as the default shell for the login node of the Nurion system. The chsh command is used for changing to a different shell.

```
$ chsh
```

&#x20;

&#x20;\- echo $SHELL is used for checking the currently used shell.

```
$ echo $SHELL
```

&#x20;\- Change the configuration files (.bashrc, .cshrc, etc.) in the home directory of a user to change the configuration settings of the shell.

&#x20;

## D. Changing User Password

&#x20;ㅇ Use the passwd command in the login node to change a user’s password.

```
$ passwd
```

&#x20;

&#x20;※Password-related security policy

ㅇ The user password must be at least 9 characters and include an alphabet, number, and special character. English words in the dictionary cannot be used.

ㅇ The user password expiration period is two months (60 days).

ㅇ The new password cannot be the same as the five previously used passwords.

ㅇ Maximum number of incorrect login attempts : 5

&#x20;\- The ID will get locked if you attempt to log in five times with the incorrect password; contact the account manager ([acccount@ksc.re.kr](mailto:acccount@ksc.re.kr)) when your account is locked.

&#x20;\- The IP address of a PC will be temporarily blocked if you attempt to log in five times with the incorrect password; contact the account manager ([account@ksc.re.kr](mailto:account@ksc.re.kr)) if needed.

ㅇ Maximum number of incorrect OTP authentication attempts : 5

&#x20;\- Contact the account manager ([account@ksc.re.kr](mailto:account@ksc.re.kr)) if you have failed to authenticate five times.

&#x20;

## E. Checking SRU Time Usage of User Account

ㅇ Access the integrated supercomputer account managing system (ISAM) to check a user’s contract information, detailed batch job usage, and total SRU time usage.

```
$ isam
```

&#x20;

## F. Job Directory and Quarter Policy

ㅇ Home directory and scratch directory information are provided below.

| **Category**                   | **Directory path** | **Capacity limit** | **No. of file limit** | **File deletion policy**                                                        | **File system** | **Backup** |
| ------------------------------ | ------------------ | ------------------ | --------------------- | ------------------------------------------------------------------------------- | --------------- | ---------- |
| <p>Home</p><p>Directory</p>    | /home01            | 64GB               | 200K                  | N/A                                                                             | Lustre          | O          |
| <p>Scratch</p><p>Directory</p> | /scratch           | 100TB              | 1M                    | <p>Automatically delete files</p><p>that have not been accessed for 15 days</p> | X               |            |

\- The home directory has limited capacity and I/O performance; thus, all computation jobs must be executed in a user’s work space of /scratch, which is the scratch directory.

\- A user’s current usage can be checked with the following commands.

```
$ lfs quota -h /home01$ lfs quota -h /scratch
```

\- A user’s job directory is applied to different policies according to use.

&#x20;

2022년 3월 22일에 마지막으로 업데이트되었습니다.
