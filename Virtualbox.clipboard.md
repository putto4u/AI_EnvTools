
버추얼박스(VirtualBox)를 우분투 VM GUI 터미널환경에서 윈도우와 상호 Copy & Paste, 클립보드 문제 해결
---

## 1단계. apt 업데이트 및 패키지 설치

```
$ sudo apt update
$ sudo apt install -y build-essential linux-headers-$(uname -r)
```

## 2단계. 게스트 확장 이미지 설치
<img width="489" height="328" alt="image" src="https://github.com/user-attachments/assets/e6710645-9c34-48f0-9bba-4bd5acf8cc53" />  

<br>  
<br> 
왼쪽 도크에서  CD 아이콘이 생성됨,  클릭
<br>  
<br> 
<img width="1127" height="546" alt="image" src="https://github.com/user-attachments/assets/be8df81f-78b1-4164-9603-a4b8933f3e24" />  
<br>  
<br> 
터미널로 열기

```
$./autorun.sh
```

재부팅하면 해결 됩니다. 
