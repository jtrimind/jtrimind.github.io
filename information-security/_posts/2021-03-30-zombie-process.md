---

title: "[정보보안기사] 좀비 프로세스(Zombie process)"
---

## 공부한 책
### 이기적 정보보안기사 필기+실기 환상의 콤비

<iframe src="https://coupa.ng/bTZNkr" width="120" height="240" frameborder="0" scrolling="no" referrerpolicy="unsafe-url"></iframe>

`파트너스 활동을 통해 일정액의 수수료를 제공받을 수 있음`  

## 좀비 프로세스(Zombie process)
- 프로세스가 종료되었으나 프로세스 테이블에 엔트리는 존재하는 프로세스
- 자식 프로세스가 종료되었으나 부모프로세스가 wait()를 해주지 않으면 발생
- defunct process라고도 함

### 좀비 프로세스 수 확인
#### `top -b -n 1 | grep zombie`
- `top`: 실시간으로 CPU 사용률을 체크해주는 명령어
  - `-b`: batch 모드
  - `-n 1`: 1번만 반복
- `grep zombie`: zombie 문자열 검색

```bash
$ top -b -n 1
top - 00:57:17 up 3 min,  0 users,  load average: 0.52, 0.58, 0.59
Tasks:   4 total,   1 running,   3 sleeping,   0 stopped,   0 zombie
%Cpu(s): 10.2 us,  9.6 sy,  0.0 ni, 79.9 id,  0.0 wa,  0.3 hi,  0.0 si,  0.0 st
KiB Mem : 16727992 total,  8343308 free,  8155332 used,   229352 buff/cache
KiB Swap: 30382520 total, 30125828 free,   256692 used.  8438928 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0    8940    320    276 S   0.0  0.0   0:00.09 init
    7 root      20   0    8940    228    184 S   0.0  0.0   0:00.00 init
```

```bash
$ top -b -n 1 | grep zombie
Tasks:   5 total,   1 running,   4 sleeping,   0 stopped,   0 zombie
```

### 좀비 프로세스 검색
#### `ps -ef | grep defunt | grep -v grep`
- `ps`: 현재 돌아가고 있는 프로세스를 알려줌
  - `-e`: 커널 프로세스를 제외한 모든 프로세스를 출력
  - `-f`: 전체 정보를 생성
- `grep defunt`: defunt 문자열 검색
- `grep -v grep`: grep 문자열은 제외
