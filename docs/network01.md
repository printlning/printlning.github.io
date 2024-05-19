---
layout: default
title: 캡슐화와 역캡슐화
parent: Network
nav_order: 1
---

# 캡슐화와 역캡슐화 - Encapsulation And Decapsulation
***


![](https://i.imgur.com/dsN4Sik.png)

인터넷에서 데이터를 주고 받을 때, 사실 데이터는 위와 같은 계층을 지납니다. 계층을 지나면서 데이터에 헤더를 붙이기도 하고 떼기도 하는데, 이것이 **캡슐화** 혹은 **역캡슐화**입니다.


![](https://i.imgur.com/VxIY6th.png)


데이터 송신(주황색) 과정에서는 헤더를 붙이면서 캡슐화를, 데이터 수신(연두색) 과정에서는 헤더를 떼면서 역캡슐화를 진행합니다.
캡슐화 과정을 살펴보겠습니다.
1. Application Layer -> Transport Layer
	**메시지**($M$)에 TCP 헤더($H_T$)를 붙여 **세그먼트**(segment)를 만듭니다.
		세그먼트 = TCP 헤더 + 메시지
	TCP 헤더는 source port, destination port 등의 정보를 담고 있습니다. 이 포트 번호를 통해 서비스를 식별할 수 있습니다.
	TCP 헤더 대신 UDP 헤더를 붙일 수도 있습니다.
2. Transport Layer -> Network Layer
	세그먼트에 IP 헤더($H_I$)를 붙여 **패킷**(packet)을 만듭니다.
		패킷 = IP 헤더 + TCP 헤더 + 메시지
	IP 헤더는 IP 주소 등의 정보를 담고 있습니다.
3. Network Layer -> Data Link Layer
	패킷에 이더넷 헤더($H_E$)와 트레일러($T$)를 붙여 **프레임**(Frame)을 만듭니다. 트레일러는 오류를 확인할 때 사용하게 됩니다.
		프레임 = 이더넷 헤더 + TCP 헤더 + 메시지 + 트레일러
	이더넷 헤더는 source MAC, destination MAC 등의 정보를 담고 있습니다. 
4. Data Link Layer -> Physical Layer
	프레임을 비트로 변환 데이터를 전송합니다.
위 단계를 역으로 수행한 것이 역캡슐화 과정입니다. 또한 여기서 메시지, 세그먼트, 패킷, 프레임은 각 계층의 데이터를 부르는 명칭이라고 생각하시면 됩니다. 

 보통 이더넷 헤더는 14 byte, IP 헤더와 TCP 헤더는 20 byte, UDP 헤더는 8 byte 입니다.
 따라서 실질적인 데이터가 시작되는 부분은 14+20+20= 54 byte 혹은 14 + 20 + 8 = 42 byte 부근이 됩니다.
