---
description: >-
  기술지원 > 지침서 > 사용법 > NURION(ENG) > User Environment > F. Job Directory and
  Quarter Policy
---

# F. Job Directory and Quarter Policy

ㅇ Home directory and scratch directory information are provided below.

| **Category**                   | **Directory path** | **Capacity limit** | **No. of file limit** | **File deletion policy**                                                        | **File system** | **Backup** |
| ------------------------------ | ------------------ | ------------------ | --------------------- | ------------------------------------------------------------------------------- | --------------- | ---------- |
| <p>Home</p><p>Directory</p>    | /home01            | 64GB               | 200K                  | N/A                                                                             | Lustre          | O          |
| <p>Scratch</p><p>Directory</p> | /scratch           | 100TB              | 1M                    | <p>Automatically delete files</p><p>that have not been accessed for 15 days</p> | Lustre          | X          |

\- The home directory has limited capacity and I/O performance; thus, all computation jobs must be executed in a user’s work space of /scratch, which is the scratch directory.

\- A user’s current usage can be checked with the following commands.

```
$ lfs quota -h /home01$ lfs quota -h /scratch
```

\- A user’s job directory is applied to different policies according to use.
