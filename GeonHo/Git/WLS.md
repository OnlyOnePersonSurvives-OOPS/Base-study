# WSL

## 1. WSL - What, Why, How?

### What?
**windows subsystem for linux**
리눅스 커널만 따올 수 있다.

### Why?
리눅스를 메인 os로 설치하거나  
가상환경안에서 돌려야 함.  

WSL
-> 윈도우를 메인 OS를 쓰며 리눅스의 장점을 가져올 수 있다!

### How?
**WSL 1**: 윈도우 커널 -> 그 위에 WSL가 돌아감. -> 리눅스 커널 위에서 윈도우 파일 접근 가능  
WSL 2: Hypervisor(윈도우 커널 아래) 위에 WSL가 돌아감. -> 윈도우 파일 접근 불가능  

둘 다 윈도우 환경에서 리눅스를 구현한다는 점은 똑같음.
구현 방법에서의 차이

performance across OS file systems: WSL1 O, WSL2 X

실제 리눅스와 비슷한건 WSL 2  


powershell 실행
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart (virtual machine 허용)

https://docs.microsoft.com/ko-kr/windows/wsl/install-win10#step-6---install-your-linux-distribution-of-choice 6단계부터  

sudo apt-get update 

## 2. Using git in WSL

리눅스의 특징 - 뿌리: Unix (서버 개발 운영체제 중 하나)  
모든 것을 키보드로 해결  

flag: -

ls -l (l: long) 길게 뽑아내라.

clear (Ctrl + l)

vim hello.c

wq (save and quit)  

vim : text editor  

gcc: c compiler (순수하게 컴파일 해라) -> 디폴트로 실행가능한 파일이 a.out으로 생긴다.  
gcc hello.c  

sudo apt-get install -y build-essential  
모든 것에 대해서 yes를 해라.   

[참고](http://linux-command.org/ko/build-essential.html)  

./a.out : 실행해라!   

./ : 현재 내가있는 폴더  

---

GitHub

git init

ls -al
하면 .git 파일 생성


git remote add origin 주소

git pull origin main
깃이 origin에서 가져올 것인데 그곳의  main branch에서 가져올 것이다.  

이제 가져온 내용을 로컬에서 수정을하고 add commit push 할 것!

파일을 만든 뒤 변경 사항 반영하기  

config --global user.name  
--global  폴더에 상관없이 commit 하는 사람은 김건호 이다.  
