Chapter4 

DHCP: Dynamic Host configuration Protocol

네크워크 관련된 정보들을 수동으로 설정하지 않아도 DHCP서버에서 자동으로 가져와서 설정해 주는 것이다.

(서브넷, 게이트 웨이, DNS)

- 사용 중에 주소를 다시 새롭게 바꿀 수도 있고 어싸인 할 수 도 있도록 한다
- 일정시간 동안 지켜서 할 수 있도록 한다.



:star: IP 주소가 없는데 DHCP server에 어떻게 접근하는 걸까?

### DHCP client-server senario

![image-20201109135141131](/Users/juhui/Library/Application Support/typora-user-images/image-20201109135141131.png)

UDP 사용

1. (자기 주소를 아직 할당 받지 못하는 경우) client는 아직 자기 주소를 모른다, 그래서 자기를 나타내는 방식으로 포트넘버를 적어 보낸다. Ex) 0.0.0.0, 68

   DHCP server주소도 잘 몰라서, 그냥 255.255.255.255로 서브넷에 있는 모든 애들한테 값을 보낸다.

   (다른 애를 갔다가 오는 것을 구분 하기 위해) transaction ID를 랜덤한 값으로 설정한다. 

2. DHCP offer

   자기 주소를 적은 다음에, 다시 브로트 캐스트 255.255.255.255,68로 보냄

   Transaction ID가 654이기 때문에 다른 애들은 해당 DHCP offer 을 쓰지 않는다.

3. DHCP request

   Src: 0.0.0.0 68 -> 아직 자기 값이 할당 받지 않아서 0.0.0.0이다.

   Dest 255.255.255.255, 67 아는데도 불구하고 브로드 캐스트 시킨다. 왜그럴까?

   transation ID는 1을 증가 해서 보낸다.

4. DHCP 사용가능 하면 이에 대한 ACK를 보내고 아니면 NACK를 보낸다.

   ->ack이 오면 다 쓰고 RELEASE를 하면 된다.

   ->nack이 오면 다시 위의 상황을 반복하면 된다.



:star:각 상태 마다 어떤 주소값을 가지는 것을 아는 것이 중요하고, Port가 새로운 역할을 하는 것을 알고 있어야 한다!@



### Network Address Translation (NAT)

:주소를 바꾸어 주는 것. 이 기술 덕분에 아직 IPv4가 계속 사용 되고 있는 것이다!

사슬망으로, 기존의 IP주소를 private network를 사용하는 것이다. 

 Local network에서는 unique 하지만 글로벌 적으로는 unique하지 않다.



Motivation: 왜냐하면, local network는 밖에 접속할 때 하나의 IP address만 있으면 되기 때문이다.

local network 서버가 있지 않고서야 밖에서 접속할 일이 없기 때문이다.(즉 글로벌리 유니크할 필요가 없음)

ISP로 부터 여러개의 아이피 주소를 받아 올 필요가 없고, 하나를 공유한다.

그렇기 때문에 내부에서 IP주소를 마음대로 바꾸어도 문제가 생기지 않는다. 또한 밖에 아이피를 바꾸는 것도 하나만 바꾸면 되어서 간단하다. 같은 맥락에서 바깥에서 안에서 보는 것이 불가능하다.(안전에 좋음)



중간에 라우터에서 NAT translation table를 사용해서 값을 나누어 준다. 



NAT is controversial:

- 라우터는 3층까지 만 건들 수 있는데, nat는 지금 포트 넘버(4층)까지 건들고 있는 중이다.
-  Src IP 와 dest IP를 건들어 버린다. (그래서 p2p를 사용하기 위해서는 무조건 NAT를 사용하지 않는 방식을 찾아야 한다.)
- NAT뒤에 있는 server에 client는 어떻게 접근 해야 하는 경우...!



NAT traversal problem

1. 미리 테이블을 만들어 놓는다. static 하게 정해 놓는 것! 바로 어디로 가야할지!(사람 손이 많이 간다)

2. Internet Gateway Device Protocol (네트워크 할때 잘 다루지 않는 부분)  사람이 해야하는 부분을 자동으로 시키는 것

3. TURN(Traversal Using Relay Around NAT), 현재 스카이프에서 사용

    = NAT뒤에 있는 서버에서  Relay의 IP 주소와 포트 넘버를 대신해서 사용하는 것이다.

   외부망에 NAT서버(relay서버)를 둔다. 

   먼저 relay 서버에 먼저 접속를 한다. 

   그 다음에 응답을 받으면 relay 서버의 IP 주소와 포트넘버를 넘겨준다.    



### ICMP(Internet control message protocol)

Error reporting , 네크워크 진단 같은 일을 한다. 

데이터 그램에 에러가 발생하면 IP는 이를 알 수가 없기 때문에 ICMP가 소스 쪽에 문제점을 알려준다. 

-> 하지만 checksum error가 나면 header쪽에 있는 source에서 문제가 있는 것이기 때문에 메세지를 보내지 않는다.

ICMP는 Ip를 통해서 보내지는 데 에러가 나면, 그냥 보내지 않는다. ICMP는 layer 3로 간주한다. 

Ping, pong이 여기에 해당한다.

TTL expried: TTL값이 영이 되어질때 버려지는 것

DF =1 이런 것! Path MTU를 찾을 때 쓰는 것

  

