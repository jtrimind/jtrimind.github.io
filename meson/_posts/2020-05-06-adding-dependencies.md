---

title:  "[Meson] 튜토리얼02 - adding dependencies"
---

meson에서 gtk 디펜던시를 추가하여 빈 화면을 보여주는 예제를 실행해본다.

## 준비물

gtk development libiraries를 설치한다. ubunutu의 경우 apt를 이용하여 설치할 수 있다.

- gtk development libiraries

```bash
sudo apt install libgtk-3-dev
```

아래와 같이 `main.c`와 `meson.build`를 작성한다.

- main.c

```c
#include<gtk/gtk.h>

int main(int argc, char **argv) {
  GtkWidget *win;
  gtk_init(&argc, &argv);
  win = gtk_window_new(GTK_WINDOW_TOPLEVEL);
  gtk_window_set_title(GTK_WINDOW(win), "Hello there");
  g_signal_connect(win, "destroy", G_CALLBACK(gtk_main_quit), NULL);
  gtk_widget_show(win);
  gtk_main();
}
```

- meson.build
 
```meson
project('tutorial', 'c')
gtkdep = dependency('gtk+-3.0')
executable('demo', 'main.c', dependencies : gtkdep)
```
## meson 빌드하기
빌드 디렉토리를 설정하지 않았다면, 아래와 같이 설정한다.

```bash
$ meson builddir
```

`builddir`로 이동하여 `ninja` 명령어를 통해 코드를 빌드한다.

```bash
$ cd builddir
$ ninja
```

```
[1/1] Regenerating build files
The Meson build system
 version: 0.13.0-research
Source dir: /home/jpakkane/mesontutorial
Build dir: /home/jpakkane/mesontutorial/builddir
Build type: native build
Project name is "tutorial".
Using native c compiler "ccache cc". (gcc 4.8.2)
Found pkg-config version 0.26.
Dependency gtk+-3.0 found: YES
Creating build target "demo" with 1 files.
[1/2] Compiling c object demo.dir/main.c.o
[2/2] Linking target demo
```

빌드된 바이너리를 실행한다.

```bash
$ ./demo
```

![demo output window](https://mesonbuild.com/images/gtksample.png)

## 참고 사이트
[Meson Tutorial](https://mesonbuild.com/Tutorial.html)
