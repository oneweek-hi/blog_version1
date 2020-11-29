Chapter 6: The Data Link Layer

6.1 introduction, services

path중에서 인접한 노드들을 연결시켜주는 커미션 채널을 link 라고 한다.

- wired link
- wireless links
- LANs

data-link layer: 링크를 통해서 하나의 노드에서 인접한 노드까지 frame을 전달 하는 , 쉽게 말하면 인접한 노드들 사이의 커뮤니케이션을 담당하는 계층이다.



Framing, link access: tailer에 CRC가 들어 있어서 에러를 체크하는 역할을 한다.  Header에는 MAC 어드레스 또는 Pysical 어드레스가 들어가게 되서 실제 해당하는 프레임을 받을 건지 말건지는 여기서 결정됨

Error control:

error detection는 주로 CRC를 쓰는데 parity bit를 가끔 쓰는 경우도 있다.

Error recovery (FEC, ARQ)레이어 투에서 잘 안하지만 

옛날에는 에러가 많아서 Link by Link로 error 체크를 했는데, 지금은 에러 발생률이 낮아 그렇게 하는게 매우 비효율 적이다. 그래서 end to end로 error recovery를 체크하는 것을 주로 사용한다.

Flow는 control은 요즘은 별로 문제가 되지 않는다. 워낙 프로세스가 빠르고 메모리가 많아서

Half duplex and full duplex 동시에 양쪽 방향 통신이 가능 한것 하프는 어는 순간 부터 한쪽만 가능한것.



#### Medium Access Control(MAC)

Broadcast networks 에서는 여러명이 동시에 보낼 수 있기 때문에 문제가 된다.

하나의 매체를 여러명의 사용자가 쉐어를 하고 있기 때문에 엑세스 할때 중재가 필요하다. 아니면 충돌이 나서 힘들다.  그래서 Medium Access Control는 다른 stations간에 전송을 어떻게 할지 코디네이션, 조절하는 기능이다.



Adaptors Communicating

데이터 링크 레이어는 NIC이라고 하는 어댑터 쪽에 구현이 되어 있다.  

![image-20201124013235607](/Users/juhui/Library/Application Support/typora-user-images/image-20201124013235607.png)



### Framing 

에러가 있는 가 없는 가를 확인 하려면 프레임 사이의 경계치를 구분해 주는 게 필요하다

프레임을 구별하기 위해서는 특별한 패턴이있어야 한다.

현재는 아래의 두개만 쓰인다.

- Bit - stuffing
- Octet stuffing



### Bit-stuffing

HDLC(High-level data link control), SDLC가 있는데 주로 전자가 더 많이 쓰임

시작과 끝이 flag로 이루어져 있다. 01111110 (중간에 1이 여섯개)

![image-20201124013923608](/Users/juhui/Library/Application Support/typora-user-images/image-20201124013923608.png)

만약 sender쪽에서 일이 여섯개라면 다섯개 이후에 무조건 영을 추가한다. 그럼 리시버쪽에서 일 다섯개 이후에 영하나는 다시 삭제 한다. 무조건 다섯개 일때마다 항상 영을 추가해서 보낸다. 그래야 값이 진짜 일이 다섯개만 연속할 경우에도 문제 없이 작동 할 수 있기 때문이다.

일반적으로 이 과정을 하드웨어 쪽에서 수행하는 역할이다. 



### Octet Stuffing 

핸들링을 소프트웨어 하기 때문에 PPP해서 HDLC와 비슷해 보지만 비트바이 비트단위로 하는게 아니라 바이트 단위로 계산한다. 

![image-20201124014432158](/Users/juhui/Library/Application Support/typora-user-images/image-20201124014432158.png)



만약 플레그 값인 ox7e가 오면 ox7d+ ox5e라고 보내고 만약 ox7d라는 값이 정말 있으면 ox5d라고 해서 보낸다.  그럼 받는 쪽에서 ox7d가 오면 일단 무조건 버리고 그 다음값에 ox20을 해서 사용하는 방식으로 계산한다. 

여기서 address, control들이 여러가지 의미가 있는데 여기서 address는 111111111는 슬레이브 주소라서 누구를 알려주는 것인데 일대일 통신이라서 별로 의미가 없다고 한다. FCS는 주로 에러 체크가를 하기 위해 CRC코드를 집어 넣는데 16비트 아니면 32bit를 사용한다.



## 6.2 error detection, correction

 에러를 핸들링하는 방식은 크게 두가지가 있다.

Method1: Forward Error Correction(FEC)

Adding redundant bits to enable the receiver to correct errors.

Method2: Retransmission 재전송 방식



#### Error control

보내야 하는 데이터만 보내는 것이 아니라 그로 부터 만들수 있는 에러를 체크할 수 있는 redundancy한 코드를 추가해서 보낸다. 그럼 리시버에는 그 관계가 성립하는지 확인하는 방식으로 진행되는 것이다.

 

![image-20201124021331032](/Users/juhui/Library/Application Support/typora-user-images/image-20201124021331032.png)

Hamming distance

임의의 두 legal한 codewords사이에 서로 다른 비트의 미니멈인 것을 말하는 것이다. 

![image-20201124021815040](/Users/juhui/Library/Application Support/typora-user-images/image-20201124021815040.png)

- 만약 해밍 디스턴스의 값이 d+1이면 d bit의 에러는 무조건 잡아 낼 수 있다.
- 만약 해밍 디스턴스 값이 2d+1 d bit의 에러는 correction을 할 수 있다. 즉 000과 111 밖에 없는데 010이 왔다! 그렇다면 받는 쪽에서는 이것은 원래 000이라고 여길 수 있는 것이다. 확률적으로 판단할 수 있는 것이다

![image-20201124135735572](/Users/juhui/Library/Application Support/typora-user-images/image-20201124135735572.png)

1,3,5가 일때 에러 체크가 가능 한데 해밍 디스턴스는 d-1일때 체크가 가능하기 때문에 여기서 해밍 디스턴스의 길이는 2인것을 알 수 있다.





#### Parity check code

![image-20201124140113318](/Users/juhui/Library/Application Support/typora-user-images/image-20201124140113318.png)

1 bit 까지는 error correction이 가능 하고(확실히 어디서 낫는지 알아서 고칠 수 있다)3 bit 까지는 error 디텍션이 가능하다. -> 3개까지는 어디서 에러가 낫는지 알 수 있기 때문에 여기서 해밍 디스턴스는 4이다. 



#### Internet Checksum

->에러 체크 기능이 좋지는 않지만 컴퓨테이션 오버헤드를 줄이면서 간단하게 체크할 수 있다. 

16비트씩 잘라 원스 컴플리먼트를 어디셔널 해서 그 결과를 bit by bit 원스 컴플리 먼트 시켜서 체크썸에 포함 시켜서 보낸다.

원스 컴플리 먼트 모든 값이 일이면,에러가 없는 경우 이고 아니면 에러가 있는 것이다.



#### Cyclic Redundancy Check(CRC)

- standard method in data link layer

  -> 가장 많이 쓰이는 에러 체크 기능, 하드웨어 생성이되고 에러도 체크한다. 단순하고 성능이 매우 좋아서 거의 에러를 다 잡는다.

- polynomial Coding 

![image-20201124141455166](/Users/juhui/Library/Application Support/typora-user-images/image-20201124141455166.png)

계산법

![image-20201124141608332](/Users/juhui/Library/Application Support/typora-user-images/image-20201124141608332.png)



R(x)에서는 modulo-2, exclusive-on으로 되어 있다.

![image-20201124142124999](/Users/juhui/Library/Application Support/typora-user-images/image-20201124142124999.png)![image-20201124142135071](/Users/juhui/Library/Application Support/typora-user-images/image-20201124142135071.png)

![image-20201124142154455](/Users/juhui/Library/Application Support/typora-user-images/image-20201124142154455.png)

하오... 출석과제...



![image-20201124162033506](/Users/juhui/Library/Application Support/typora-user-images/image-20201124162033506.png)그렇다면 리시버 쪽에서는 에러가 났는지 않났는지 어떻게 알 수 있을까?

P(x)=P(x)x^r+R(x)로 보내면, 가는 동안 에러가 나면 에러는 R(x)=P(x)+E(x)

1<u>0</u>001이 보냈데 밑줄 친 부분이 에러가 나서 11001이 되면 이것은

=10001+(익스클루시브 or)01000으로 표현이 가능하다. E(x)=01000이 되는 것으로 에러의 값을 표시하는것 이다. 만약 에러가 없으면 E(x)=0이다.

P(x)는 G(x)로 나누었을때 나누어 떨어지기 때문에 받은 값을 G(x)로 나누어서 나머지가 영이면 에러가 안난 걸로 간주하고 영이 아니면 에러가 난것이라고 간주하는 것이다



#### CRC

- G(x) =(x+1)G1(x)로 표시 될 수 있다면 odd 갯수의 에러는 모두 다 체크할 수 있다.

- burst error는 연속적으로 일어나는 에러, 처음에러가 났을때의 마지막 났을때의 차이. 

  이것이 R비트 보다 작으면, R비트 보다 작으면 다 잡아 낼 수 있다. 나머지로 다 나오기 때문에!

- 크더라도 대부분 잡아낼 수 있다. 잡아 내는 확률이 99퍼센트다. G(x)를 어떻게 만드는야가 매우 중요하다.

  ![image-20201124170451770](/Users/juhui/Library/Application Support/typora-user-images/image-20201124170451770.png)



