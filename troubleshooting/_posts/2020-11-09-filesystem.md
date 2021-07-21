---

title:  "[해결법] fatal error: filesystem: No such file or directory"
---

C++의 `<filesystem>`은 C++17에 도입되었다.  
따라서 C++17을 지원하지 않는 컴파일러의 경우 사용할 수 없다.  
혹은 C++17을 사용하도록 하는 컴파일러 옵션이 빠졌을 수도 있다.  

## 해결법
### 컴파일러 버전 확인
1. gcc 버전확인
	```bash
	$ gcc --version
	gcc-7 (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
	```
2. gcc 버전이 8보다 작으면 최신 버전의 gcc을 설치한다.
   ```bash
   $ sudo apt install gcc-8 g++-8
   ```
3. 최신 버전의 gcc을 default로 설정한다.
   ```bash
   $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 700 --slave /usr/bin/g++ g++ /usr/bin/g++-7
   $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 800 --slave /usr/bin/g++ g++ /usr/bin/g++-8
   ```
4. 설정된 gcc 버전 확인
   ```bash
   $ gcc --version
   gcc (Ubuntu 8.4.0-1ubuntu1~18.04) 8.4.0
   ```

### 컴파일 옵션 확인
gcc, g++ 사용시 `--std=c++17` 옵션이 주어졌는지 확인한다.
