Chapter4 

IPv6

원래  IPv4(32bit)여서 더 이상 쓸수 있는 IP가 없을 것이라고 예상되어 만들어진 것 이다.

128 bit address를 할당하여서 매우 많은 아이피를 만들 수 있다. 여유있게 쓸 수 있어서 더 많은 기능들을 넣을 수 있다. 

그리고 header길이를 가변이 아니라 40byte로 고정 시켰다.

![image-20201109154726877](/Users/juhui/Library/Application Support/typora-user-images/image-20201109154726877.png)



Version표시 하는 것은 양쪽다 필요하다

header length는 가변이 아니기 때문에 필요가 없어짐

type of Service -> Traffic Class 바뀜 상대적인 개념으로써 누구를 먼저 처리해 줘야 하는지!

total length-> Payload Length : total length는 원래 header 길이 + Payload인데 헤더길이가 이미 정해 졌기 때문에 Payload만 알면 된다. 

Identification, Flags, Fragment Offset 등 fragement에 관련된 것은 다 사라짐. 

TTL -> Hop Limit 

Protocol -> Next Header

Header Checksum 은 없어졌다. -> IP에서 하라고 됨 사실 잘 모름....!

Source Address: 4byte(32bit) -> 16byte(128bit) 4배가 증가..

옵션은 없어졌음..

= 결론적으로 40byte으로 고정 되어 있다. 



Alignment changed from 32(4B 단위) to 64bits (8B 단위)..!

version부분 빼고는 조금 씩 다 바뀐 것을 알 수 있다. 



Next Header: 체인식으로 값을 처리하기 쉽게 하기 위함.

![image-20201109162245345](/Users/juhui/Library/Application Support/typora-user-images/image-20201109162245345.png)



### Fragmentation

반드시 보내는 쪽에서 나누어 받는 쪽에서 합친다. 중간 라우터에서는 하지지 못한다.

너무 처리 할 수 없을 만틈 큰 값이 오면 버려지고, 이것을 소스에게 말함.

그래서 꼭 path MTU를 꼭 찾아야 함!



하지만, 안될 경우가 있기 때문에 이를 할 수 있는 Next header를 활용해서 할 수 있는 방법을 남겨 두었다. 하지만 권장하지 않는다. 



### Address Notation

너무 길어서...Colon-Hexadecimal로 표현하기로 함.

0을 줄일 수 있다.

Ipv4f랑 호환도 된다.



![image-20201109163031744](/Users/juhui/Library/Application Support/typora-user-images/image-20201109163031744.png)

Anycast : 가장 가까운 미러 쪽으로 갈 수 있도록 하는 것.

브로트 캐스트 주소느 없음

![image-20201109163043906](/Users/juhui/Library/Application Support/typora-user-images/image-20201109163043906.png)





### Transition From IPv4 to IPv6

1. 두개를 다 지원하는 방식이 있다.  

2. Tunneling

   터널을 지날때 형식을 맞춰줘서 보낸다음 다시 벗기는 형식으로 교류하는 것.

   ![image-20201109163718316](/Users/juhui/Library/Application Support/typora-user-images/image-20201109163718316.png)

   

아직까지도 IPv4에서 IPv6로 넘어 가는데 아직도 남았따....

IoT는 IPv6를 쓴다. 



