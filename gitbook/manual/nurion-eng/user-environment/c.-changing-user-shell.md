---
description: 기술지원 > 지침서 > 사용법 > NURION(ENG) > User Environment > C. Changing User Shell
---

# C. Changing User Shell

\- BASH is provided as the default shell for the login node of the Nurion system. The chsh command is used for changing to a different shell.

```
$ chsh
```

\- echo $SHELL is used for checking the currently used shell.

```
$ echo $SHELL
```

\- Change the configuration files (.bashrc, .cshrc, etc.) in the home directory of a user to change the configuration settings of the shell.
