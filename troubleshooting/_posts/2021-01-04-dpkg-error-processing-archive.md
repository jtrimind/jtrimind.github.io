---

title:  "[해결법] dpkg: error processing archive xxx.deb (--unpack):"
---

## 현상
package가 broken되어 안내대로 `sudo apt --fix-broken install`을 하였음에도 에러가 발생할 수 있다.  
overwrite를 하려고 했으나 실패한 것으로 보인다.  

```
$ sudo apt --fix-broken install
...
Preparing to unpack .../xxx.deb ...
Unpacking xxx-c (0.10.3) ...
dpkg: error processing archive /var/cache/apt/archives/xxx.deb (--unpack):
 trying to overwrite '/usr/share/man/man8/xxx.8.gz', which is also in pac
kage xxx 0.10.9
Errors were encountered while processing:
 /var/cache/apt/archives/xxx.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

## 해결법
아래와 같이 `--force-overwrite` 옵션으로 강제 설치한 뒤, 다시 `sudo apt --fix-broken install`를 한다.  

```bash
$ sudo dpkg -i --force-overwrite /var/cache/apt/archives/xxx.deb
$ sudo apt --fix-broken install
```
