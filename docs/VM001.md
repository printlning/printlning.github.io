---
layout: default
title: Kali Linux 설치
parent: VM
nav_order: 1
---

# Kali Linux 설치
***
<br/>

ISO파일을 이용해서 VMware에 공격자 역할인 Kali Linux를 설치해보겠습니다.

### ISO 파일 준비
OS설치를 위한 ISO파일부터 다운로드하겠습니다. 칼리 리눅스를 설치할 것이기 때문에  [kali.org](https://www.kali.org/) 에 접속해줍니다.

![](https://i.imgur.com/DeGh1Ey.png)
2024년 5월 기준으로는 위와 같은 화면이 나옵니다. DOWNLOAD를 클릭합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/A41Z8L5.png)
Installer Images를 선택합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/3cUaIU6.png)
본인 컴퓨터에 맞는 파일을 다운받아줍니다. 저는 64-bit를 다운받았습니다.

<br/>
<br/>
<br/>

### 설치
ISO 파일을 이용해서 칼리 리눅스를 설치해보겠습니다.
상단 메뉴에서 File > New Virtual Machine... 을 선택하거나, Ctrl + N을 눌러줍니다.

![](https://i.imgur.com/AAR4Cbi.png)
Typical과 I will install the operating system later. 를 선택합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/GntMPtk.png)
칼리 리눅스는 데비안 기반이므로 Version은 Debian으로 선택합니다. 이름과 저장 경로는 원하는 대로 해주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/Fb5ac9Y.png)
용량을 설정해주고, Store virtual disk as a single file을 선택합니다. 그리고 Customize Hardware를 눌러주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/dmZItvo.png)
프로세서 코어 수를 2로 설정해주시고, 아까 다운받은 ISO 파일도 설정하고 닫아주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/pXCiuNG.png)
![](https://i.imgur.com/8KRgij1.png)
가상머신을 작동하면 위와 같은 설치 화면이 나옵니다. Graphical install을 선택하고 한국어로 설정해주세요.

<br/>
<br/>
<br/>

![](https://i.imgur.com/GAE5MDu.png)
호스트 이름과 도메인 이름을 설정합니다. 저는 기본값으로 뒀습니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/t4hrNCd.png)
![](https://i.imgur.com/lVAkFap.png)
계정과 비밀번호를 설정합니다. 파티션 방법은 자동-디스크 전체 사용으로 선택했습니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/6NCeu04.png)
![](https://i.imgur.com/dkn4Mwx.png)
![](https://i.imgur.com/T1gWP3J.png)
파티션 나누기를 마치고 바뀐 사항을 디스크에 쓰기를 선택하고 부트 로더도 설치해줍니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/IIKcYnr.png)
부트로더 장치는 /dev/sda를 선택하고 설치가 완료되면 계속을 눌러 부팅합니다.

<br/>
<br/>
<br/>

### 설정
설치를 마쳤으니 간단한 설정을 해주겠습니다.

##### root 계정 비밀번호 설정
![](https://i.imgur.com/tofDHy3.png)
아까 설치할 때 만들었던 계정과 비밀번호로 로그인합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/YNuBoX8.png)
```shell
sudo su
passwd
```
sudo su -> 현재 계정의 비밀번호를 입력합니다.
passwd -> 원하는 대로 root 계정의 비밀번호를 설정한 뒤, 리부팅해줍니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/MxmYeTf.png)
아까 설정한 비밀번호를 이용해 root로 로그인합니다.

<br/>
<br/>
<br/>

##### 화면 꺼짐 설정
![](https://i.imgur.com/DIvJXmI.png)
Setting > Power Manager > Display 에서 화면이 꺼지지 않도록 해줍니다. 필수는 아니지만 여러 개의 가상머신을 한번에 켜뒀을 때, 화면이 꺼지면 불편할 수 있어서 설정해줬습니다.

<br/>
<br/>
<br/>

##### 업데이트, 업그레이드
![](https://i.imgur.com/KlS2gwn.png)
![](https://i.imgur.com/DcMNN9F.png)
```shell
vi /etc/apt/sources.list
apt-get -y update
apt-get -y upgrade
```
먼저 vi로 sources.list 파일을 수정합니다.
	i 를 입력해 입력 모드로 들어갑니다.
	다섯 번째 줄의 주석을 해제합니다.
	Esc 를 입력해 명령 모드로 전환합니다.
	:wq 를 입력해 저장하고 종료합니다.
그 후 업데이트와 업그레이드를 해줍시다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/ilFLxFP.png)
업그레이드가 끝나면 위와 같은 창이 나올 수 있는데, Tab을 눌러 Ok를 선택합니다.

<br/>
<br/>
<br/>

##### IP 고정

이제 IP 주소를 설정해주겠습니다.

```shell
vi /etc/network/interfaces
```
![](https://i.imgur.com/Ly0NqG9.png)
사진처럼 interfaces 파일에 아래와 같은 내용을 추가해줍니다. IP는 원하는 대로 설정하셔도 됩니다. 저는 250으로 설정했어요.
```
auto eth0
iface eth0 inet static
address 192.168.10.250
netmask 255.255.255.0
gateway 192.168.10.2
network 192.168.10.0
broadcast 192.168.10.255
dns-nameservers 168.126.63.1
```
이때 본인의 subnet address에 맞게 수정해서 추가해야 합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/ODe9cB5.png)
상단 메뉴에서 Edit > Virtual Network Editor 에 들어가 subnet address를 확인할 수 있어요.
위 경우에서는 192.168.190.0 이므로, 아래와 같이 수정하면 됩니다.
```
auto eth0
iface eth0 inet static
address 192.168.190.250
netmask 255.255.255.0
gateway 192.168.190.2
network 192.168.190.0
broadcast 192.168.190.255
dns-nameservers 168.126.63.1
```

<br/>
<br/>
<br/>

![](https://i.imgur.com/g2UCPyT.png)
ifconfig로 IP가 잘 설정됐나 확인합니다.

<br/>
<br/>
<br/>

![](https://i.imgur.com/0GmR9oW.png)
위 사진처럼 ping이 잘 실행되면 성공입니다!

