---

title:  "[해결법] error while loading shared libraries: libreadline.so.6"
---

`error while loading shared libraries: libreadline.so.6: cannot open shared object file: No such file or directory`이라는 에러가 발생하는 경우가 있다.

## 해결방법
1. libreadline가 없다면 설치한다.  
   ```bash
   $ sudo apt-get install libreadline-dev
   ```
2. `/lib/x86_64-linux-gnu/`에 `libreadline.so.7`은 있는데 `libreadline.so.6`은 없다면 링크를 걸어준다.
   ```bash
   $ cd /lib/x86_64-linux-gnu/
   $ sudo ln -s libreadline.so.7.0 libreadline.so.6
   ```

`libreadline.so.6`를 찾아 설치해주는 것이 가장 좋으나 `libreadline.so.7`를 링크로 걸어줘도 잘 동작했다.

## 참고사이트
- [libreadline-so-6-issue-in-ubuntu-18-04](https://askubuntu.com/questions/1168787/libreadline-so-6-issue-in-ubuntu-18-04)
