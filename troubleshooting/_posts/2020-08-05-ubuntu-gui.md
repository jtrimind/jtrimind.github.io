---

title:  "[해결법] Ubuntu GUI 안나옴"
---


Ubuntu를 켰는데 원래 보던 GUI 화면이 안나오고 CUI 콘솔 화면만 나타날 때가 있다.  

## 해결법
### ubuntu-desktop 문제
1. ubnutu-desktop 설치 확인
	사용자의 실수로 ubuntu-desktop이 삭제되었을 수도 있다.  
	ubuntu-desktop이 설치되어있는지 확인하자.  

	```bash
	$ dpkg -l | grep ubuntu-desktop
	# 설치가 되어있는 경우 아래와 같이 나타남
	# ii  ubuntu-desktop 1.417.4 amd64 The Ubuntu desktop system
	```
2. ubuntu-desktop 설치
	만약 ubuntu-desktop이 설치가 안되어 있다면, ubuntu-desktop을 설치한다.
	
	```bash
	$ apt-get install -y ubuntu-desktop
	```
3. reboot
	재부팅을 하면 GUI가 나타나는 것을 확인할 수 있다.
	```bash
	$ reboot
	```
