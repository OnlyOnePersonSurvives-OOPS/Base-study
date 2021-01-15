# [Git_Handbook](https://guides.github.com/introduction/git-handbook/)

## 1. 버전 컨트롤이란?

### VCS
팀 프로젝트를 진행함에 있어 버전의 history 변화를 살펴야 한다.  
프로젝트를 진행함에 따라 테스트를 수행하고 버그를 고치고 새로운 코드를 추가할 때 언제라도 어느 버전을 복구할 수 있다는 확신을 가져야 한다. 

- 무슨 변화를 줬는가?
- 누가 바꿨는가?
- 언제 바꿨는가?
- 바꾼 이유는 무엇인가?

## 2. distributed version control system이란?

Git은 DVCS의 가장 흔한 예다.

centralized version control systems과 달리  
DVCS는 central repository에 지속적으로 연결할 필요가 없다. (비동기식)  

### 버전 컨트롤이 없다면??
-> 시간 지연, 여러가지 사본, 불필요한 작업을 맞닥뜨리게 된다.
ex) 최종.pptx, ㄹㅇ최종.pptx, 진짜최종.pptx

독립적인 작업을 하더라도 VCS가 제공하는 정보(what, who, where, why)를 통해 서로 보조를 맞출 수 있게 도와준다!

## 3. Why Git?

- 변화, 결정, 진행상황에 대한 timeline을 한 곳에서 볼 수 있다.  
프로젝트 history에 접근한 순간부터, 프로젝트를 이해하고 기여하는데에 필요한 context를 가지게 된다.

- 기존 소스코드를 온전히 놨둔채로 언제든지 협업을 진행할 수 있다.  
branch를 이용하면, 안전하게 코드 변경 제안을 할 수 있다.

- 커뮤니케이션 장벽을 허물고 온전히 작업에 집중할 수 있게 도와준다.

## 4. repository란?

Repository: 프로젝트와 관련된 폴더, 파일, history를 아우른다.
commits: snapshot형태로 나타나는 파일 history (version). 연결리스트 형태로 존재한다.
branches: 여러 개발 라인, commit은 branch로 구성될 수 있다.  
lightweight pointers to commits in history that can be easily created and deprecated when no longer needed.  

레포지토리의 copy를 가진 누구라도 전체 codebase와 history에 접근할 수 있다. (DVCS기 때문)  

brance 이용  
-> 개발자는 mainline을 손상시킬필요 없이 코드 수정, 버그 fix를 할 수 있다.  

## 5. 기본적인 Git 명령어

copy, create, change, combine

- git init: 새로운 Git repository를 initialize하고 기존 디렉토리 추적을 한다.  
기존 폴더에 버전 컨트롤에 필요한 내부 자료 구조를 저장할 숨겨진 subfolder를 추가한다.  

- git clone: 이미 원격으로 존재하는 프로젝트의 local 복사본을 생성한다.  
복사본엔 프로젝트의 모든 파일, history 및 branch가 포함된다.

- git add: 변경 사항을 staging area에 올린다. git은 개발자의 codebase변화를 추적할 수 있지만, 그러기 위해선 변경 사항을 staging area에 올리고 snapshot을 만들어 프로젝트 history에 추가해야 한다. 이 명령어는 두 가지 단계중 staging 단계에 포함된다. staged된 어느 변경사항이든 다음 snapshot와 프로젝트 history의 일부가 될 것이다. 스테이징 및 커밋을 수행하면 개발자는 코드와 작업 방식을 변경하지 않고도 프로젝트 기록을 완벽하게 제어할 수 있다.

- git commit: snapshot을 프로젝트 기록에 저장하고, 변경 사항 확인 프로세스를 완료한다.  
사진 한장을 찍는 것과 비슷하다.  
git add로 올려진(staging) 어떤 것이든 git commit을 통해 snapshot의 일부가 된다.  


### summary

git init: git을 디렉토리에 초기화  
git add 'file_name': 해당 파일을 버전관리할 준비된 상태로 만듦. (staging area에 올린다.)
git commit -m 'message': staging area에 있는 파일들을 '버전'으로 만듦.
