# Domain Name System
## 1. IP주소와 hosts
- 각각의 **인터넷에 연결된 기기**들을 모두 싸잡아서 **host**라고 부름
- 내 기기로 **다른 host로 접근하기 위해선 IP주소가 필요**하다(**전화번호: 93.184.216.34**)
- 근데, 내 컴퓨터에 있는 **'hosts'파일에 'IP주소-도메인이름'을 저장**해놓으면 이 이름으로 접근할 수 있다(**전화번호부: 93.184.216.34 - example.com**)

## 2. 도메인 이름과 보안
- hosts파일은 굉장히 굉장히 굉장히 변조에 취약하다
- 누가 이름은 놔두고, 전화번호를 바꿔버리면 엉뚱한 사람한테 전화를 거는 상황이 나올 수 있따...

## 3. DNS가 등장한 이유
- DNS가 등장하기 전엔 'Stanford Research Institute'라는 기관에서 전세계의 hosts파일을 관리했다 - 다운받아서 쓰는 방식 ㅇㅇ
-> 문제점: 시간이 너무 오래 걸림, hosts파일이 너무 커... 등등 많은 문제가 있었음

## 4. DNS의 원리
1. 서버가 **DNS서버에 이름-IP주소 등록**
2. 클라이언트가 **도메인이름을 브라우져에 입력하면 DNS서버에서 자동으로 해당 IP를 매칭**해줌
3. 이 IP를 가지고 클라이언트는 서버로 접속할 수 있음

## 5. Public DNS의 사용
기기가 원하는 도메인 이름의 IP주소를 알아낼 수 있는 이유는 기기에 연결된 'local DNS서버'덕분임. local DNS는 통신사가 자동으로 설정을 해주지만, 이를 다른 DNS서버로 변경하는것이 가능함. 이번에는 public DNS의 개념과 어떻게 변경하는지를 알아볼것이당
- 통신사(ISP)마다 기본적으로 갖추고있는 DNS서버를 가지고 있고, 이는 DHCP로 인해 자동으로 설정된다
-> 근데 이러한 방식은 내가 접속한 사이트의 목록이 유출될 가능성이 있다 (사생활침해 ㅠ)
- 그래서 DHCP를 안쓰고 **직접 DNS서버 주소를 입력하면 해당 DNS서버를 쓸 수 있음**
-> 이때 **쓸 수 있는 공공 DNS서버가 바로 'public DNS'서비스**임 
-> ex. Google이 운영하는 8.8.8.8 서버, cloudFlare의 1.1.1.1

## 6. 도메인 이름의 구조 (DNS Internal)
DNS서버는... 전세계에 한 개만 있나...? nope. **서버는 전세계에 분산**되어 있다. 어떻게 이게 가능할까?
blog.example.com.(마지막 점은 생략 가능)  
-> blog : **sub**  
-> .example : **Second-level**  
-> .com : **Top-level**  
-> . : **Root**  
이렇게 각 부분으로 나뉘는데, **각 부분을 담당하는 DNS서버가 다 따로따로 있다**  
- **상위 level의 DNS서버는 직속 하위 level DNS서버의 목록과 IP주소**를 모두 알고 있다 (Root > Top > Second > sub)
- 결국 전체 **도메인이름의 IP주소는 sub DNS서버가 알고 있다**. 그런데 저어어어언 **세계에 퍼져있는 sub DNS서버 중 어떤놈이** 내가 원하는 IP주소를 저장하고 있는지 **모르기때문에, Root부터 탐색해 나가는 것!!!!!**
- **DNS서버를 이용하는 모든 기기는 'Root name server'가 어디있는지 알고 있다**
~~~
//C는 Client
C->Root : blog.example.com. 은 어떤 IP야?
Root->C : 음.. 내가 .com을 맡고있는 서버들은 알아, 얘들 목록 알려줄게
C->Top : 니가 .com을 맡고있다며? blog.example.com.이거 누구야?
Top->C : 어... 내가 .example.com담당하는 서버를 알고 있어, 얘들 목록을 알려줄게
C->Second : ㅅㅂ 니가 .example.com담당이구나? blog.example.com.이거 누구냐?
Second->C : 내가 알지는 못하지만 blog.example.com.을 가지고 있는 애들은 알고 있지. 가장 가까운 친구 알려주께
C->sub : 내놔
sub->C : 여기있습니다.
~~~
대충 이런 식이다...ㅎ.ㅎ

## 7. 도메인 이름 등록 과정과 원리 (DNS Register)
**Registrant(서버)**가 exmple.com이라는 도메인 이름을 가지고 싶다면 어디에서 등록해야할까?
- **ICANN(관리자)**: 모든 Root name server를 관리하는 비영리 단체 - a.root-servers.net
- **Registry(등록소)** : Top-level domain을 관리하는 곳 - a.gtld-servers.net
- **Registrar(등록대행자)** : 등록소에 대신 등록해주는 곳 - a.이창하-servers.net(직접 만들거나, 등록대행자가 빌려주는 서버를 사용하면 됨)
- 모든 DNS서버에는 **[도메인-태그-위치/IP] 형식으로 정보가 저장**되어 있다

- 등록 과정
0. Root name server에는 등록소의 top-level서버 정보가 저장되어 있다 [ com NS a.gtld-servers.net ]
1. 등록자가 등록대행자에게 example.com을 등록해주세요~ 라고 말한다 
2. 등록대행자가 등록소에 example.com을 등록해주세요~ 라고 말한다.
3. 등록대행자가 빌려준 name server에 example.com과 그 IP주소를 저장한다 [ example.com A 123.123.123.123 ]
4. 등록소에서는 example.com이 어느 name server에 저장되어있는지 저장한다 [ example.com NS a.이창하-servers.net ]  
-> Registrant(123.123.123.123) : example.com을 내 도메인 네임으로 삼고싶다  
-> 등록대행자가빌려준nameserver(a.이창하-servers.net) : example.com A 123.123.123.123 [4]  
-> 등록소의TopLevel네임서버(a.gtld-servers.net) : example.com NS a.이창하-servers.net [3]  
-> ICANN의ROOTNameServer(a.root-servers.net) : com NS a.gitld-servers.net [2]  
(5. 통신사가설정한/또는내가 설정한 PUBLIC DNS에는 ROOTNameServer가 저장되어있다  
->기기에 등록된 DNS server : . NS a.root-servers.net [1] )

## 8. nslookup 사용법(도메인 이름 정보 조회)
nslookup은 도메인이름에 대한 정보를 조회하는데 사용되는 대표 도구이다. 자세한 사용법을 알아보자
- 이 도메인의 실제 IP는 무엇인가? 알고싶지 않니...?
- (window OS) cmd에서 진행됩니다
~~~ 
> nslookup -type=a www.google.com //구글 IP주소가 궁금해요 (type=a는 생략가능한 default값이다)
     서버:    one.one.one.one  //어떤 DNS서버를 통해 필요한 서버를 조회했나 알려주는 항목
     Address:  1.1.1.1

    권한 없는 응답: //1.1.1.1 DNS서버가 www.google.com에 권한이 없다
                                   (얘말고 캐쉬가 대답한거면 권한없다고 뜬다)
    이름:    www.google.com
     Addresses:  2404:6800:4005:810::2004
               172.217.24.132

> nslookup -type=ns www.google.com //실제 IP를 가지고 있는 서버가 누구인지 궁금해요~
     서버:    one.one.one.one
     Address:  1.1.1.1

     google.com
        primary name server = ns1.google.com //이놈이 IP주소를 가지고 있구나

> nslookup www.google.com ns1.google.com //캐쉬말고 니가 직접 대답해볼래?
     서버:    ns1.google.com
     Address:  216.239.32.10

     // 권한없는 응답이라고 뜨지 않는다!!
     이름:    www.google.com
     Addresses:  2404:6800:4004:80c::2004
               172.217.31.164
~~~ 

## 9. 나의 도메인 이름 만들기
freenom.com이라는 무료 도메인 서비스를 이용해서, 내 도메인 이름을 만들어보장
- 등록대행자로서 freenom.com을 사용할것이다아
- Record를 등록해서 sub도메인, 메일 도메인 등을 모두 만들 수 있다

## 10. DNS record & CNAME
도메인 이름에 대한 정보 한건 한건을 DNS Record라고 합니다. 여기서는 DNS Record의 타입(A,NS 등등)을 살펴보고, IP주소가 아닌 도메인 이름에 대한 별명을 지정하는 방법으로서 CNAME Record 타입에 대해서 알아봅니다
- CNAME Record
-> DNS Record타입 중에 이름-이름을 연결하는 도메인을 의미함
-> 이걸 이용하면 github에 올린 page의 url을 내 입맛에 맞게 바꿀 수 있음
