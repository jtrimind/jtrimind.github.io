---

title:  "[해결법] ERROR: The Nouveau kernel driver is currently in use by your system."
---

NVIDA driver를 설치하다가 위와 같은 에러 로그와 함께 설치에 실패할 수도 있다.  
이는 Nouveau kernel driver가 사용 중이라 설치를 할 수 없다는 메시지다.  

## 해결방법

1. `/etc/modprobe.d/blacklist.conf`을 열어 `blacklist nouveau`을 추가한다.
2. 아래 명령어로 nouveau를 비활성화시킨다.
	```terminal
	$ echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
	$ sudo update-initramfs -u
	```
3. 재부팅한다.

## 참고 사이트
- [Ubuntu NVIDIA Graphic Driver 설치(nouveau kernel 문제 등)](http://jinyongjeong.github.io/2016/11/22/ubuntu_graphic_driver_install/)
