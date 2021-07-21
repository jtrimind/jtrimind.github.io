---

title:  "[해결법] Unable to negotiate with xxx port xxx: no matching key exchange method found"
---

## 로그

```
Unable to negotiate with xxx port xxx: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
```

## 해결방법
`~/.ssh/config`에 해당 알고리즘을 추가한다.

```
Host 192.168.0.1
HosKeyAlgorithms +diffie-hellman-group14-sha1
```



