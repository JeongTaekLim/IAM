# Introduction

서브넷(Subnet)의 사전적 의미는 특정지역에서 관리되는 IP 영역을 몇개의 영역으로 나눠서 관리하는 것이다.
eg. 부하가 심한 서비스 A1, A2를 묶어서 관리하고 부하가 심하지 않은 B1, B2를 묶어서 관리함으로써 A1의 네트워크장애가 B1에 미치지 않도록 운영가능하다.

# Net mask와 Subnet mask
Net mask는 인터넷 상에서 서브넷이 필요하지않은 경우 사용되는 마스크로, 어떤 비트들이 NET 영역이며, 어떤 부분이 HOST영역인지를 계산(해석) 하기위해 사용된다. 그러므로 각각의 클래스에 대한 넷마스크는 다음과 같이 정의될 것이다.

|클래스 |	넷마스트 | 비트|
|-----|--------|----|
|A	| 255.0.0.0	| 11111111.00000000.00000000.00000000|
| B	| 255.255.0.0 |	11111111.11111111.00000000.00000000|
|C	|255.255.255.0 |	11111111.11111111.11111111.00000000|

서브넷 마스크는 단지 위의 넷 마스크에서 8bit 더 확장한 경우로써, 어떤한 서브넷으로 전달되어야하는지를 해석하기 위한 목적으로 사용된다. 예를들어 B 클래스 IP에 대한 서브넷마스크는 (11111111.11111111.11111111.00000000) 이 될것이다. 서브넷마스크는 가장 마지막의 8bit 의 비트들을 조작함으로써 만들어주게 된다.

즉, 11111111.11111111.11111111.10000000(255.255.255.128)로 서브넷마스크 비트를 확장시켰다면 2^1(2) 개의 서브네트웍을 구성할 수 있다.

예를들어 C 클래스 영역을 가지며 Network address가 203.211.5.0일때 서브넷마스크를 255.255.255.128로 구성했다면, 각각 사용가능한 host address의 범위가 203.211.5.1 ~ 203.211.5.128, 203.211.5.129 ~ 203.211.5.254인 2개의 서브네트웍을 구성할 수 있다.

https://www.joinc.co.kr/w/Site/Network_Programing/Documents/SubNetWorking