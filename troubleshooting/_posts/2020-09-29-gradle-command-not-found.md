---

title:  "[해결법] gradle: command not found"
---

gradle이 설치되어있지 않으면 `gradle: command not found`와 같은 에러 메시지가 생성된다.  

## 해결법

```bash
# Gradle 설치
$ sudo mkdir /opt/gradle
$ wget https://services.gradle.org/distributions/gradle-6.1.1-all.zip
$ sudo unzip -d /opt/gradle gradle-6.1.1-all.zip
$ rm gradle-6.1.1-all.zip

# 설치 확인
$ ls /opt/gradle/gradle-6.6.1
# bin  init.d  lib  LICENSE  NOTICE  README

# 환경변수 추가
# `~/.bashrc`의 가장 마지막줄에 `export PATH=$PATH:/opt/gradle/gradle-6.6.1/bin`를 추가하고 터미널을 다시 여는 것을 권장
$ export PATH=$PATH:/opt/gradle/gradle-6.6.1/bin

# 설치 확인
$ gradle -v
# ------------------------------------------------------------
# Gradle 6.6.1
# ------------------------------------------------------------
```

## 참고 사이트
- [Gradle 설치 페이지](https://gradle.org/install/)
