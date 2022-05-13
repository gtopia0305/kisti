---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > Miscellaneous Manual > A. Desktop
  Virtualization (VDI)
---

# A. Desktop Virtualization (VDI)

Desktop virtualization (VDI) is a remote desktop service for executing applications requiring high-performance graphic processing in a Windows environment. VDI is capable of pre- and post-processing of engineering interpretation in a virtual machine (VM) without transferring files by linking with the filesystem of Nurion. Interpretation (solving) is not supported in VMs; it can be executed in the Nurion system through terminal access programs (putty, etc.) within VMs.

For each of the eight VMs, 3.0 Ghz 4-core CPU, 32 GB memory, and 8 GB GPU memory are allocated . To support multiple users with a limited number of VMs, a user is automatically logged off when idle for two hours while connected to VMs.

Currently, Abaqus 2020 and Ansys 2020 R2 are supported as engineering interpretation applications. Supported applications will be expanded in the future depending on the user demand.

To use the VDI service, complete the application form below and send it to account@ksc.re.kr. A temporary password will be issued after approval by an administrator. The temporary password needs to be changed when accessing the VM for the first time, and it does not get linked with the Nurion password.

Errors or questions during use can be inquired by going to Contact menu on the KSC website (https://www.ksc.re.kr).

&#x20;

**ㅇ Application form for VDI service**

I would like to apply for the Nurion system VDI service.

\- Nurion ID :

\- Usage period :

\- Application to use :

&#x20;

**ㅇ How to access VM**

Download and install Horizon Client appropriate for a user’s environment from the website below to access VM.\
[https://my.vmware.com/en/web/vmware/downloads/info/slug/desktop\_end\_user\_computing/vmware\_horizon\_clients/horizon\_8](https://my.vmware.com/en/web/vmware/downloads/info/slug/desktop\_end\_user\_computing/vmware\_horizon\_clients/horizon\_8)

&#x20;

After installing Horizon Client, complete the settings as shown below.

![](<../../../../.gitbook/assets/After installing Horizon Client, complete the settings as shown below..png>)

Settings menu on the top right corner - SSL Configuration

![](<../../../../.gitbook/assets/Settings menu on the top right corner - SSL Configuration.png>)

Select “Do not check server ID certificate” and then click OK.

![](<../../../../.gitbook/assets/Select “Do not check server ID certificate” and then click OK..png>)

New Server - enter nurion-vdi.ksc.re.kr and then click Connect.

![](<../../../../.gitbook/assets/New Server - enter nurion-vdi.ksc.re.kr and then click Connect..png>)

Enter the Nurion ID and OTP number to log in.

![](<../../../../.gitbook/assets/Enter the Nurion ID and OTP number to log in..png>)

Enter the temporary VDI password issued by the administrator at the first access (the same ID as the Nurion system is used, but the password is different)

![](<../../../../.gitbook/assets/Enter the temporary VDI password issued by the administrator at the first access (the same ID as the Nurion system is used, but the password is different).png>)

When accessing VM, image initialization and user profile creation require approximately 3 min

![](<../../../../.gitbook/assets/When accessing VM, image initialization and user profile creation require approximately 3 min.png>)

Once VM access is completed, home01 and scratch of the Nurion system are automatically mounted.  The NFS of the desktop is executed when not mounted or an error occurs.\
The C:\ drive and control panel are blocked from access because of security reasons; a user is automatically logged off when idle for two hours

