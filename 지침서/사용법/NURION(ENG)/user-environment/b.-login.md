---
description: 기술지원 > 지침서 > 사용법 > NURION(ENG) > User Environment > B. Login
---

# B. Login

\- Users can gain access through the login node (nurion.ksc.re.kr) of the Nurion system (refer to the node configuration below).

\- Default character set (encoding) is in unicode (UTF-8).

\- Only SSH, SCP, SFTP, and X11 are allowed to access the login node.

① Unix or Linux environment

```
$ ssh -l nurion.ksc.re.kr [-p 22]
```

e.g.) When user ID is x123abc

```
$ ssh -l x123abc nurion.ksc.re.kr
 or 
 $ ssh -l x123abc nurion.ksc.re.kr -p 22
```

② Windows environment

\- Run Xming to execute the X environment

※ Download the program for free from the internet and install it.

![](<../../../../.gitbook/assets/프로그램은 인터넷을 통해 무료로 다운로드 후 설치.png>)

\- Use ssh access programs such as putty or SSH Secure Shell Client

※ Programs can be downloaded for free from the internet.

※ Host Name : nurion.ksc.re.kr, Port : 22, Connection type : SSH

![](<../../../../.gitbook/assets/Host Name  nurion.png>)

※ ssh -> X11 tap -> check "Enable X11 forwarding"

※ X display location : localhost:0.0

![](<../../../../.gitbook/assets/x display location 0.0.png>)

※ If the system cannot be accessed because of a DNS caching problem, clear all cache (run ipconfig/flushdns in command prompt) and access again.

```
C: ipconfig /flushdns
```

③ Sending/receiving files

\- Access the Datamover node (nurion-dm.ksc.re.kr) through an FTP client to send/receive files (refer to the node configuration below).

```
$ ftp nurion-dm.ksc.re.kr
 or 
 $ sftp [user ID@]nurion-dm.ksc.re.kr [-p 22]
```

\- In the Windows environment, use FTP/SFTP client programs such as WinSCP that are distributed for free to access the system.

![](<../../../../.gitbook/assets/윈도우 환경에서는 WinSCP와 같이 무료로 배포되고 있는.png>)

\* File Transfer Protocol (FTP) is used, where files can be sent without entering the OTP

\* Secure-FTP (SFTP) is used, where the OTP must be entered when sending files (safer than FTP).

④ Node configuration
