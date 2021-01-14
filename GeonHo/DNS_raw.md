---

ip주소를 기억하기 너무 힘듦.

→ DNS 탄생

DNS의 핵심은 DNS server : 수많은 ip address의 domain 이름이 저장되어 있음

모든 운영체제엔 hosts라는 파일이 있음

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8502df4-09c7-48a8-9074-5ec4a25f38c2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8502df4-09c7-48a8-9074-5ec4a25f38c2/Untitled.png)

hosts는 전화번호부

---

Security

hosts파일이 변조되는 것을 방지한다. ← 백신 사용 ( hosts가 변경되는 것을 예민하게 지켜봄 )

ex) fishing

---

DNS 이전의 상황

example.com으로 도메인을 바꾸면 192.~~으로 모든 사람들이 들어오게 할 순 없을까.

Stanford Research Institute → 전세계의 hosts파일을 관리했음.

→ 수작업 : 비용, 시간 비효율

→ Stanford Research Institute의 hosts를 다운받지 않으면 업데이트 안됨.

1983 Domain Name System 탄생

---

Domain Name System Server

집 컴퓨터에 랜선 꽂거나 와이파이 접속하면 domain name system server의 ip주소가 자동으로 세팅됨.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc0afc77-59cd-42c8-9be2-55f3a8123fdf/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc0afc77-59cd-42c8-9be2-55f3a8123fdf/Untitled.png)

이전엔 hosts가 파일로 관리되었지만

DNS 서버를 이용한다면 hosts가 수정되는 순간 클라이언트 컴퓨터에도 바로 적용됨

---

Public DNS

DNS server도 ip 주소를 가지고 있다.

client 컴퓨터도 특정 domain으로 접속하기 위해선 DNS server의 ip주소를 통해 DNS server로 들어가서 그 domain에 해당하는 ip 주소를 가져와야한다.

ISP (Internet Service Provider)

ex) KT, Sk broad band ...

ISP에 연결하는 순간 DNS server 세팅을 해준다.

그렇다는 것은 우리가 요청한 domain들을 ISP는 알 수 있다는 뜻. → 정보 유출 가능성

→ Public DNS server

자신이 신뢰하는 DNS서버로 바꿀 수 있다. 

---

도메인 이름의 구조

DNS Internal

DNS의 원리

domain

blog.example.com

→ 이 도메인 각각을 담당하는 서버 컴퓨터들이 존재한다.

계층구조

Root domain은 Top-level domain들을 알고 있어야함.

직속 하위파트만 알고 있다. 

Client computer는 root name server의 ip 주소가 뭔지를 알고 있음.

root : .com을 전담하는 name server들을 알려줄게.  (Top-Level)

---

DNS 등록하는 법

- 등록자(registrant)

등록 대행자에게 example NS a.iana-servers.net 이라는 정보를 등록 대행자에게 보내주면 등록 대행자가 등록소에게 이 정보를 알려준다.

- ICANN : 전 세계의 IP주소를 관리, Root name server(a(~m).root-servers.net)들의 관리자.

인터넷 체계의 관리자, 거대한 비영리 단체

전세계의 Top-level domain server들의 주소를 기억한다.

com NS(name server)은 a.gltd-servers.net이라는 주소에 저장되어 있다.

- 등록소(Registry) : 기관들

Top-level domain(a.gtld-servers.net)들을 관리 ex) .com.

등록소는 우리가 쓸 ip 주소를 알고 있어야 한다.

example.com a.iana-servers.net이라는 정보를 저장해둔다.

- 등록대행자(Register) : Top-level domain에 등록자의 ip를 대신 등록해주는 매개 역할 담당.

authoritative name server라는 컴퓨터에 우리의 ip 주소가 등록된다.

서버 한대를 마련해야 한다. (대체로 등록 대행자가 대신 마련해줌)

a.iana-servers.net

example.com A 93.184.216.34라는 정보를 저장해둔다.

- ISP로 client가 example.com에 접속하는 여정

 ISP엔 자동적으로 NS a.root-servers.net이라는 정보를 가지고 있다.

※모든 DNS server는 '**.** root name servers'의 주소가 뭔지에 대한 정보를 가지고 있다. (통신의 시작)

Client → ISP → Root name server

→ ISP → Top-level server

→ ISP → authoritative name server

→ ISP → Client : 아이피 주소를 알려줌.

Client → Registrant 접속

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fac21c6-ba9f-4c5b-aa06-62033c49d5e8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8fac21c6-ba9f-4c5b-aa06-62033c49d5e8/Untitled.png)

---

- nslookup (도구)

DNS 관련 정보들을 수집하는 방법.

example.com의 ip는 무엇인가?

cmd

nslookup example.com

=

nslookup -type=a example.com 

1.1.1.1은 DNS server의 ip

DNS server는 클라이언트가 example.com 에 접속할 때마다 요청 요청 요청 하는 작업을 거치지 않고 최초에 cashe로 저장해둔다.

cashe가 응답한 것이라면 권한 없는 응답으로 답이 온다.

example.com의 권한을 갖고 있는 서버는 authoritative name server

권한있는 서버에 직접 물어보고 싶다면

nslookup -type=ns example.com

→ nameserver에 대한 정보를 가져올 수 있음

name server에 직접 물어보고 싶다면

nslookup example.com a.iana-servers.net

---

나만의 도메인 이름 장만하기

[Freenom - A Name for Everyone](https://www.freenom.com/en/index.html?lang=en)

---

DNS record중의 하나인 CNAME

DNS  record : DNS server에 저장하는 domain 이름에 대한 정보 한 건

ex) dns4u.ga A 52.132.~ (DNS record type은 A)

CNAME 별명을 지어주는 것

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f85b9e7-606b-4784-9bcf-2e958f5e5a74/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f85b9e7-606b-4784-9bcf-2e958f5e5a74/Untitled.png)

a

b

c    → CNAME → example.com → 192.0.1.1

d

e

아이피 주소가 바뀌어도, a, b, c, d, e는 변함없이 example.com 으로 연결된다.

---
