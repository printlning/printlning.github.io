---
layout: default
title: SYN Flooding
parent: Network
nav_order: 2
---

# SYN Flooding
***

SYN Flooding은 3-way handshake에서 취약점을 이용합니다. 이해를 위해 3-way handshake가 무엇인지부터 알아보겠습니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/x6j4fw0.png)
위 사진이 TCP 연결을 위한 3-way handshake 과정입니다.
1. 클라이언트가 서버에게 접속을 요청하는 SYN을 보냅니다.
2. Listen 상태인 서버가 SYN을 받고 ACK + SYN를 클라이언트에게 보냅니다. 앞서 보낸 SYN에 대한 ACK라 sequence number는 x+1이 됩니다.
3. SYN + ACK를 받은 클라이언트가 서버에게 ACK를 보내고 TCP 연결이 성립됩니다.

<br/>

이때 SYN을 받은 서버는 Backlog queue에 연결 요청 정보를 저장하고, 정상적으로 처리를 완료하면 삭제합니다. 즉, ACK를 받기 전까지 일정 시간 동안 요청을 저장해 둡니다. 
만약 SYN만 보내고 서버의 SYN + ACK에 응답하지 않으면 서버는 일정 시간 동안 응답을 기다려야 합니다. SYN flooding은 이 점을 이용해 Backlog queue를 가득 채우는 공격입니다. 
단시간에 많은 요청을 보내고, 응답하지 않으면서 서버의 Backlog queue를 채워 다른 연결을 받을 수 없게 만드는 것입니다.

<br/>

이제 SYN Flooding이 무엇인지는 알았는데, 그렇다면 어떻게 서버의 SYN + ACK에 응답을 하지 않을 수 있을까요?
Source IP를 스푸핑해 다른 주소로 바꿔주면 됩니다. 그러면 IP 주소가 가짜이기 때문에 서버에 ACK 패킷을 보내지 않습니다.

<br/>
<br/>
<br/>

## 실습
Kali Linux: 192.168.10.250
CentOS 7: 192.168.10.50
***
Kali Linux를 이용해 CentOS 7에 SYN Flooding 공격을 해보겠습니다. 

<br/>
<br/>
<br/>

![](https://i.imgur.com/aZ89q8S.png)
Kali Linux 터미널에서 아래와 같이 입력해주세요.
```shell
hping3 --rand-source 192.168.10.50 -S -p 80 --flood
```
--rand-source로 랜덤한 source IP를 이용해 CentOS 7의 80번 포트로 SYN Flooding 공격을 진행합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/jYlqm04.png)
CentOS 7을 보면 공격때문에 먹통이 된 것을 확인할 수 있습니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/5JciQQu.png)
공격을 받았을 때의 상황을 확인하기 위해 시스템 모니터를 띄워보겠습니다. CentOS 7에서 gnome-system-monitor를 실행해주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/vdyDy5S.png)
공격을 받기 전의 평화로운 상태입니다.

<br/>
<br/>
<br/>

다시 Kali Linux에서 공격한 뒤
```shell
hping3 --rand-source 192.168.10.50 -S -p 80 --flood
```
아까와 같은 상황을 막기 위해 최대한 빨리 CTRL+C를 눌러 종료해주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/CfMAXsD.png)
그리고 CentOS 7에 가서 모니터를 확인합니다. 공격을 받은 시점부터 네트워크 사용량이 올라간 것을 확인할 수 있습니다. 

<br/>
<br/>
<br/>

```shell
netstat -tuna
```
마찬가지로 공격을 종료한 뒤 최대한 빨리 netstat로 상태를 확인해보면

<br/>
<br/>
<br/>

![](https://i.imgur.com/xZ6ok1F.png)
다양한 IP로부터 많은 SYN을 받은 것을 알 수 있습니다.

<br/>
<br/>
<br/>

Wireshark를 이용한 패킷 캡처도 살펴보겠습니다.
![](https://i.imgur.com/SWx2U7x.png)
netstat으로 확인했던 것과 동일하게 다양한 IP로부터 받은 SYN 패킷을 볼 수 있습니다. SYN Flooding공격이기 때문에 응답으로 ACK 패킷을 받지 못 한 것 또한 확인할 수 있습니다.

<br/>
<br/>
<br/>

## 대응
1. Backlog queue 확대
	SYN Flooding은 Backlog queue를 채우는 공격입니다. 그러므로 단순하게 queue의 사이즈를 늘리는 것도 방어에 도움이 될 수 있어요.
2. 보안 솔루션
	UTM 등의 솔루션을 이용해 비정상적인 접근을 탐지/차단할 수 있습니다.
3. SYN Cookie 사용
	쿠키를 사용하면 SYN 요청을 받았을 때 Backlog queue에 저장하지 않습니다. 대신 SYN+ACK를 보낼 때 쿠키값을 포함해 전송합니다. 그 후 쿠키값을 이용해 ACK를 검증하고, 연결을 수립합니다. 연결 요청에 바로 리소스를 할당하지 않으니 공격으로부터 네트워크를 보호할 수 있지만, 처리 속도가 느려질 수 있다는 단점이 있습니다.
