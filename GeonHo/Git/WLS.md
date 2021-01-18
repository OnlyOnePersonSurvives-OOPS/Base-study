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
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart




## 2. Using git in WSL
