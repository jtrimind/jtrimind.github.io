---

title:  "[Meson] 튜토리얼01 - the humble begining"
---

meson을 이용하여 "Hello there."을 출력하는 예제를 실행해본다.

## 준비물
아래와 같이 `main.c`와 `meson.build`를 작성한다.

- main.c

```c
#include<stdio.h>

int main(int argc, char **argv) {
  printf("Hello there.\n");
  return 0;
}
```

- meson.build
 
```meson
project('tutorial', 'c')
executable('demo', 'main.c')
```
## meson 빌드하기
meson에서는 별개의 빌드 디렉토리를 생성해야 한다.  
일반적으로 최상위 소스 디렉토리에 빌드 디렉토리를 생성하여 설정한다.  
튜토리얼에서는 `builddir`이라는 빌드 디렉토리를 설정하여 생성한다.

```bash
$ meson builddir
```

명령어를 실행하면 아래와 같은 로그가 출력된다.  

```
The Meson build system
Version: 0.52.0
Source dir: /home/user/meson_test
Build dir: /home/user/meson_test/builddir
Build type: native build
Project name: tutorial
Project version: undefined
C compiler for the host machine: cc (gcc 7.5.0 "cc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0")
C linker for the host machine: GNU ld.bfd 2.30
Host machine cpu family: x86_64
Host machine cpu: x86_64
Build targets in project: 1
Found ninja-1.9.0 at /home/user/anaconda3/bin/ninja
Generating targets:
Writing build.ninja:
```

`builddir`로 이동하여 `ninja` 명령어를 통해 코드를 빌드한다.

```bash
$ cd builddir
$ ninja
```

빌드된 바이너리를 실행한다.

```bash
$ ./demo
```

```
Hello there.
```

## 참고 사이트
[Meson Tutorial](https://mesonbuild.com/Tutorial.html)
