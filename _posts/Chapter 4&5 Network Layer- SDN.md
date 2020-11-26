#### Chapter 4&5 Network Layer: SDN

원래는 control plane랑 data plane이 하나로 묶여 있었는데 이것을 두개로 나누어서 각각 따로 역할을 하도록 하였다.

![image-20201123144944270](/Users/juhui/Library/Application Support/typora-user-images/image-20201123144944270.png)

CA들이 먼저 데이터를 다 수집하고 remote controller로 보내면, 거기서 forwarding table을 만들어서 다시 전달해주면 해당 테이블에 맞게 forwrding을 한다.

**control plane:** 정보를 모아서 플로딩 테이블을 만들어서 보내주는 것

**data plane:** 플로팅 테이블을 전달받은 뒤, 그에 따라 직접 forwarding 하는 것

Open Protocol, 공개된 프로토콜로 저 둘 사이에 동작을 시키도록 하면 신생업체 들이 들어오기 쉬워져서 경쟁이 가속화 될 수 있음. 그럼 더 합리적인 가격으로 이용이 가능해 진다. 



### Software defined networking(SDN)

기존에는 라우터에 하드웨어 적인 것과 소프트 웨어적인 것이 한번에 있었다.

그리고 라우터와는 또 다른 네트워크의 기능들인 middleboxes가 많다.

(firewalls, load balancer, NAT boex, L3/L4)

=> 하지만 SDN이 있으면 이 위의 것을 다 한번에 구현 할 수 있다.



Why a logically centralized control plane?

- network를 management하기 좀 더 쉽다. 

  각각 라우터의 misconfigurations에서 발생하는 문제들을 줄일 수 있다.(왜냐면 한 쪽에서 다시 주니깐)

  traffic flows를 제어를 할때 굉장히 다양하게 제어 할 수 있는 가능성이 생긴다.

- Table-based forwarding 시키는 것이 라우터들을 원하는 기능으로 프로그래밍해서 사용 할 수 있다. centralized되서 이런 기능등을 동작 하기 쉽다.

- open 인터페이스를 가지고 하면 조금 더 다양하게 여러사람들이 사용 가능



### Traffic engineering: difficult traditional routing

![image-20201123150924260](/Users/juhui/Library/Application Support/typora-user-images/image-20201123150924260.png)

![image-20201123151007876](/Users/juhui/Library/Application Support/typora-user-images/image-20201123151007876.png)

원래는 W에 왔을때 어떤 것은 바로 Z로 Y로 이렇게 나누어서 보낼 수가 없었다. 그런데 어떤 경우 다른 목적에 따라 이렇게 할 수 있어야 한다. 기존이 라우터로는 거의 불가능하다. SDN는 굉장히 많은 옵션으로 이렇게 하는 게 가능하게 만든다.  

코스트가 다른 경우 로드 발란스를 적용 시킬 수 없다. 



Network-contorl apps:

- Brains of control

SDN controller(network OS)

- 네트워크의 상태 정보를 받아서 유지를 시킨다. 

- 퍼포먼스를 위해 분산시스템, 확장성, fault- tolerance를 만들어야 한다.

  

What's OpenFlow?

기존의 하드웨어를 가지고 실제 SDN을 만들 수 있는 아주 실용적인 접근 방법



오픈 쁠로우도 하나의 SDN의 일종이다. a kind of SDN

이게 2008년도에 나옴. 실제로 기존에 있던 하드웨어를 가지고 실제로 SDN을 만들수 있음. 

#### 오픈 플로우 프로토콜에 대해서 처음으로 표준화된것임. 

- first standard communications protocol

SDN이 만들어진 첫번째 개념은 오픈 플로우 프로토콜에서 부터 시작됨



 Flow: 흐름, 데이네이션 Ip address에 따라 결정 되는 것이 아니라,  L2, L3, L4 등의 굉장히 많은 종류로 정의된 하나의 패킷 흐름을 플로우 라고 한다. 굉장히 가변적!

어떤 특별한 그룹에 속해 있는 패킷의 흐름이 flow이다!

![image-20201123154020296](/Users/juhui/Library/Application Support/typora-user-images/image-20201123154020296.png)

앞에 여러가지 조합들이 매치가 되었을 때 어떤 액션을 취하는 가가 flow table의 역할! =>룰의 정보를 잘 조합해서 Action를 선택하고 통계를 누적 시킨다.

![image-20201123154139849](/Users/juhui/Library/Application Support/typora-user-images/image-20201123154139849.png)



![image-20201123154533128](/Users/juhui/Library/Application Support/typora-user-images/image-20201123154533128.png)





#### OpenFlow Switch

![image-20201123155053797](/Users/juhui/Library/Application Support/typora-user-images/image-20201123155053797.png)

- 각각의 테이블을 다 만들기에는 너무 커지기 때문에 multiple match를 사용함
- Group table를 만들어서 어떤 그룹에 속하면 걔들을 그룹별로 처리 할 수 있겠금.
- 통계치를 다 모아서 지연을 시키는 등 traiffc shaping을 한다. Meter table..

![image-20201123180145922](/Users/juhui/Library/Application Support/typora-user-images/image-20201123180145922.png)

Multiple match사용하면 n^2이 아니아 n+nk로 줄일 수 있게 되는 것이다. 





