---

title:  "[Anaconda] conda list - 현재 환경의 package 확인"
---

`conda list`를 통해 conda environment에서 link된 package를 확인할 수 있다.

```
usage: conda list [-h] [-n ENVIRONMENT | -p PATH] [--json] [-v] [-q]
                  [--show-channel-urls] [-c] [-f] [--explicit] [--md5] [-e]
                  [-r] [--no-pip]
                  [regex]
```

## 예시
- 전체 패키지 확인
```terminal
$ conda list
# packages in environment at /home/anaconda3:
#
# Name                    Version                   Build  Channel
_ipyw_jlab_nb_ext_conf    0.1.0                    py37_0  
_libgcc_mutex             0.1                        main  
_py-xgboost-mutex         2.0                       cpu_0
```

- `grep`을 이용한 특정 패키지 확인
```terminal
$ conda list | grep pillow
pillow                    6.2.0            py37h34e0f95_0
```
