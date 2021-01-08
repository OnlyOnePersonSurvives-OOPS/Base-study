---

공유기를 통한 웹서버 구현 방법을 배우는 수업

IPv4 → 42억개의 아이피 주소(전화번호) 표현 가능

핸드폰, 컴퓨터, IoT 통신 기기들이 기하급수적으로 늘어나자

IPv6라는 새로운 아이피 주소 체계 개발

기존의 주소체계 IPv4를 아껴쓰기 위한 노력 중 하나가 공유기

Router

통신사와 하나 계약 + 공유기 구매

WAN + LAN, LAN, LAN, LAN

케이블을 공유기에 꼽으면

통신사로 부터 받은 아이피 주소는 이제 공유기의 것이 된다.

WAN (Wide Area Network)

LAN (Local Area Network)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6db1b232-6d59-46ad-b0d9-f4bc01f7ffe6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6db1b232-6d59-46ad-b0d9-f4bc01f7ffe6/Untitled.png)

---

Network Address Translation

→ 이로인해 사설 아이피 쓰고 있는 기기들이 외부 인터넷에 접속할 수 있게 됨

192.168.0.4에서 쏜 정보를 NAT가 59.6.66.238로 바꿔주어서 위키피디아에게 전송

이는 클라이언트 관점

그렇다면 서버 입장으로서

외부의 클라이언트가 공유기를 통해 자신의 웹 서버에게 요청을 보낼려면 어떻게 해야하는가

---

포트

하나의 컴퓨터엔 여러 서버가 설치될 수 있다.

Ip가 컴퓨터에 접속하기 위한 주소라면

Port는 서버들 중 하나에 접속하기 위한 주소이다.

0 ~ 65535 포트

22 - SSH

80 - http → 80번은 웹이 쓰도록 정해져있는 포트  → 컴퓨터에 웹서버 깔면 기본적으로 80에 연결되도록 설정되어 있다.  Listening (연결되어 있다.)

0~1023 → Well-Known port → 예약된 포트. 우리가 맘대로 쓸 수 없음.

서버 한대 더 깔면

8080 port 에 웹 서버(관습적으로)

누군가 http:// ~ .com 으로 접속하면

80port로 연결됨 왜? http니까

8080에 접속하고 싶다면?

http:// ~ .com: 8080 

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06dad2f3-b576-4f3a-8206-763084d3dd53/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06dad2f3-b576-4f3a-8206-763084d3dd53/Untitled.png)

---

Port Forwarding

119.193.137.27으로 접속했을 때

172.30.1.10으로 접속하도록 해야됨

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/363ebe62-1d20-402f-9d59-27fc33e86630/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/363ebe62-1d20-402f-9d59-27fc33e86630/Untitled.png)

외부에서 119.193.137.27:8081으로 접속하면 → 172.30.1.10:80으로 안내해주기

공유기가 안내자 역할을 해줌.

→ router의 설정을 바꿔줘야함

---

유동 아이피와 고정 아이피

유동 아이피 → 부족한 아이피를 타계하기 위한 수단

ISP (Internet Service Provide)

→ 통신사

ISP와 계약을 맺고 케이블 꽂으면 IP address가 붙음

유동 아이피 : 주어진 아이피가 계속해서 바뀌는 것  (돌려막기)

휴먼 아이피 → 한달 뒤 → 아이피가 동적으로 바뀜

안쓰는 사람의 것을 회수 → 신규 유저에게 부여

단점! → 외부 브라우저가 기존 아이피에 접속했다가 나중에 접속했을 땐 전과 다른 곳으로 갈 수 있음.

고정 아이피 : 2~3만원 더 비쌈

---

DHCP

(Dynamic Host Configuration Protocol)

→ 아이피 주소 자동으로 설정하는 것 (also 서브넷 마스크, DNS...)

공유기에 연결하면 자동으로 세팅 됨.

이러기 위해선 DHCP server가 필요함 (공유기에 내장되어 있음)

인터넷 기기엔 DHCP client가 기본적으로 설치되어 있음.

고유한 식별자

8c:85:90:0c:e3:cc → mac Address / Physical Address

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d1125df1-1763-437d-957b-6106dbcbf67a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d1125df1-1763-437d-957b-6106dbcbf67a/Untitled.png)

자동으로 받아오면 DHCP client가 빈칸에 채워 놓는 식.
