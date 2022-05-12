---
description: 기술지원 > 지침서 > 사용법 > NURION(ENG) > User Environment > D. Changing User Password
---

# D. Changing User Password

ㅇ Use the passwd command in the login node to change a user’s password.

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
