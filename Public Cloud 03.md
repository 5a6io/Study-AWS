# Public Cloud 03

## Review

배포하려면 서버에 codedeploy agent가 설치되어 있어야 함.

pipeline이 중계자 역할을 함.

CIDR을 잘못 설정하면 마이그레이션해야 함.

Routing Policy → Simple, Weighted, Latency, Failover, Geolocation

## Route 53 Policy 구성 실습

### Simple

> eu ip만 넣고 dns로 접속 시도.
> 

![Untitled](Public%20Cloud%2003/Untitled.png)

> us ip 추가
> 

![Untitled](Public%20Cloud%2003/Untitled%201.png)

> cloud9 ide에서 확인
> 

```bash
dig simple.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> simple.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 18452
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;simple.st17.cj-cloud-wave.com. IN      A

;; ANSWER SECTION:
simple.st17.cj-cloud-wave.com. 1 IN     A       3.127.217.245
simple.st17.cj-cloud-wave.com. 1 IN     A       44.201.57.172

;; Query time: 40 msec
;; SERVER: 10.0.0.2#53(10.0.0.2)
;; WHEN: Wed Jul 24 01:33:32 UTC 2024
;; MSG SIZE  rcvd: 90
```

> load balancer dns 추가
> 

![Untitled](Public%20Cloud%2003/Untitled%202.png)

![Untitled](Public%20Cloud%2003/Untitled%203.png)

```bash
dig simple.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> simple.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 57558
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;simple.st17.cj-cloud-wave.com. IN      A

;; ANSWER SECTION:
simple.st17.cj-cloud-wave.com. 60 IN    A       43.202.80.223
simple.st17.cj-cloud-wave.com. 60 IN    A       15.165.125.111

;; Query time: 0 msec
;; SERVER: 10.0.0.2#53(10.0.0.2)
;; WHEN: Wed Jul 24 01:39:12 UTC 2024
;; MSG SIZE  rcvd: 90
```

### Weighted

![Untitled](Public%20Cloud%2003/Untitled%204.png)

![Untitled](Public%20Cloud%2003/Untitled%205.png)

![Untitled](Public%20Cloud%2003/Untitled%206.png)

![Untitled](Public%20Cloud%2003/Untitled%207.png)

![Untitled](Public%20Cloud%2003/Untitled%208.png)

### Latency

![Untitled](Public%20Cloud%2003/Untitled%209.png)

![Untitled](Public%20Cloud%2003/Untitled%2010.png)

![Untitled](Public%20Cloud%2003/Untitled%2011.png)

> 서울 리전에 생성한 cloud9에서 dig 명령 입력
> 

```bash
dig latency.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> latency.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 40580
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;latency.st17.cj-cloud-wave.com.        IN      A

;; ANSWER SECTION:
latency.st17.cj-cloud-wave.com. 60 IN   A       15.165.125.111
latency.st17.cj-cloud-wave.com. 60 IN   A       43.202.80.223

;; Query time: 0 msec
;; SERVER: 10.0.0.2#53(10.0.0.2)
;; WHEN: Wed Jul 24 02:03:55 UTC 2024
;; MSG SIZE  rcvd: 91
```

> 버지니아 리전의 web-server ec2의 session manager에 접속해서 dig 명령어 수행
> 

```bash
dig latency.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> latency.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50077
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;latency.st17.cj-cloud-wave.com.        IN      A

;; ANSWER SECTION:
latency.st17.cj-cloud-wave.com. 1 IN    A       44.201.57.172

;; Query time: 10 msec
;; SERVER: 10.20.0.2#53(10.20.0.2)
;; WHEN: Wed Jul 24 02:08:21 UTC 2024
;; MSG SIZE  rcvd: 75
```

> 프랑크푸르트 리전의 web-server ec2의 session manager에 접속해서 dig 명령어 수행
> 

```bash
dig latency.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> latency.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12276
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;latency.st17.cj-cloud-wave.com.        IN      A

;; ANSWER SECTION:
latency.st17.cj-cloud-wave.com. 1 IN    A       3.127.217.245

;; Query time: 0 msec
;; SERVER: 10.30.0.2#53(10.30.0.2)
;; WHEN: Wed Jul 24 02:10:32 UTC 2024
;; MSG SIZE  rcvd: 75
```

### Failover

> 상태 검사기 생성
> 

![Untitled](Public%20Cloud%2003/Untitled%2012.png)

![Untitled](Public%20Cloud%2003/Untitled%2013.png)

![Untitled](Public%20Cloud%2003/Untitled%2014.png)

![Untitled](Public%20Cloud%2003/Untitled%2015.png)

![Untitled](Public%20Cloud%2003/Untitled%2016.png)

> 레코드 생성
> 

![Untitled](Public%20Cloud%2003/Untitled%2017.png)

![Untitled](Public%20Cloud%2003/Untitled%2018.png)

> 서울 리전 web-server 강제종료.
> 

```bash
pgrep streamlit >> 87149
kill -9 87149
```

![Untitled](Public%20Cloud%2003/Untitled%2019.png)

![Untitled](Public%20Cloud%2003/Untitled%2020.png)

> 서울 리전이 비정상으로 되어 버지니아 리전으로 넘어감.
> 

![Untitled](Public%20Cloud%2003/Untitled%2021.png)

> 서울 리전 web-server 실행.
> 

```bash
streamlit run streamlit-project/main.py --server.port 80 &
```

![Untitled](Public%20Cloud%2003/Untitled%2022.png)

![Untitled](Public%20Cloud%2003/Untitled%2023.png)

### Geolocation

> 레코드 생성
> 

![Untitled](Public%20Cloud%2003/Untitled%2024.png)

![Untitled](Public%20Cloud%2003/Untitled%2025.png)

![Untitled](Public%20Cloud%2003/Untitled%2026.png)

> default local dns
> 

```bash
dig geolocation.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> geolocation.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2187
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;geolocation.st17.cj-cloud-wave.com. IN A

;; ANSWER SECTION:
geolocation.st17.cj-cloud-wave.com. 60 IN A     43.202.80.223
geolocation.st17.cj-cloud-wave.com. 60 IN A     15.165.125.111

;; Query time: 10 msec
;; SERVER: 10.0.0.2#53(10.0.0.2)
;; WHEN: Wed Jul 24 02:39:11 UTC 2024
;; MSG SIZE  rcvd: 95
```

> 미국 dns로 변경 후 시도
> 

```bash
dig geolocation.st17.cj-cloud-wave.com

; <<>> DiG 9.16.48-RH <<>> geolocation.st17.cj-cloud-wave.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 543
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;geolocation.st17.cj-cloud-wave.com. IN A

;; ANSWER SECTION:
geolocation.st17.cj-cloud-wave.com. 1 IN A      44.201.57.172

;; Query time: 210 msec
;; SERVER: 12.195.1.194#53(12.195.1.194)
;; WHEN: Wed Jul 24 02:43:18 UTC 2024
;; MSG SIZE  rcvd: 79
```

→ local dns를 미국 dns로 변경하여 dig 명령어를 실행했을 때 버지니아 리전 EC2의 Public IP가 반환됨.

> 지리적 위치와 지리적 접근성 차이
> 
- **지리적 위치 라우팅 (Geolocation Routing):**
    - 이 기능은 클라이언트의 위치에 따라 다른 리소스 집합을 제공합니다.
    - 예를 들어, 서로 다른 대륙에 있는 사용자에게 서로 다른 서버 그룹을 할당할 수 있습니다.
    - 사용자의 IP 주소에 따라 특정 지역 또는 국가에 있는 사용자를 특정 리전으로 라우팅할 수 있습니다.
- **지리적 접근성 라우팅 (Geoproximity Routing):**
    - 이 기능은 여러 지리적 위치를 고려하여 가장 가까운 리소스에 사용자를 라우팅합니다.
    - 이는 사용자와 가장 물리적으로 가까운 AWS 리전 또는 엣지 로케이션으로 사용자를 라우팅하여 응답 속도를 최적화할 수 있습니다.
    - GeoDNS 기능을 통해 사용자의 위치에 따라 동적으로 가장 최적의 리전으로 라우팅할 수 있습니다.

## AWS Cloud Network Service

### VPC Peering

B와 A와 연결을 하고 A와 C만 연결했을 때 B와 C는 연결이 되어있지 않으므로 통신이 되지 않음. → B와 C도 연결을 해야 함.

### VPC Peering 구성 실습

1. 네트워크 테스트 서버 생성

```bash
sh cloud-wave-workspace/scripts/create_ec2_instance.sh 
```

```bash
VPC found: vpc-0abbfbad7b66952c1
PRIVATE_SUBNET_01 found: subnet-0799ea63204010ed4
PRIVATE_SUBNET_02 found: subnet-01ebf0bf8530d58c8
Key pair created successfully: /home/ec2-user/.ssh/lab-edu-key-network.pem
VPC found: sg-0c8a06f231dacee0f
Security Group Rule created successfully: SSH
Security Group Rule created successfully: ICMP
NETWORK_EC2_01 created successfully: 10.0.40.220, i-06d19a6f2453092a2
NETWORK_EC2_02 created successfully: 10.0.41.54, i-06c49bbd03bdaeeec
{
    "IamInstanceProfileAssociation": {
        "AssociationId": "iip-assoc-0f0fb6f828338f3b6",
        "InstanceId": "i-06d19a6f2453092a2",
        "IamInstanceProfile": {
            "Arn": "arn:aws:iam::339712890055:instance-profile/lab-edu-role-ec2",
            "Id": "AIPAU6GDXKTDVSBHQ57TU"
        },
        "State": "associating"
    }
}
{
    "IamInstanceProfileAssociation": {
        "AssociationId": "iip-assoc-04137a99788006479",
        "InstanceId": "i-06c49bbd03bdaeeec",
        "IamInstanceProfile": {
            "Arn": "arn:aws:iam::339712890055:instance-profile/lab-edu-role-ec2",
            "Id": "AIPAU6GDXKTDVSBHQ57TU"
        },
        "State": "associating"
    }
}
Configuration created successfully: /home/ec2-user/.ssh/config
```

1. VPC Peering Resource 생성

![Untitled](Public%20Cloud%2003/Untitled%2027.png)

→ 피어링 연결 탭을 누른 후 수락 대기 중인 리소스의 연결을 수락함.

> lab-edu-rtb-pri-01 routing table 수정.
> 

![Untitled](Public%20Cloud%2003/Untitled%2028.png)

![Untitled](Public%20Cloud%2003/Untitled%2029.png)

> lab-edu-rtb-2nd-pri routing table 수정. (명시적 서브넷: 2nd-pri-01)
> 

![Untitled](Public%20Cloud%2003/Untitled%2030.png)

![Untitled](Public%20Cloud%2003/Untitled%2031.png)

```bash
ping 10.10.40.228

PING 10.10.40.228 (10.10.40.228) 56(84) bytes of data.
64 bytes from 10.10.40.228: icmp_seq=1 ttl=127 time=0.925 ms
64 bytes from 10.10.40.228: icmp_seq=2 ttl=127 time=0.178 ms
64 bytes from 10.10.40.228: icmp_seq=3 ttl=127 time=0.162 ms
64 bytes from 10.10.40.228: icmp_seq=4 ttl=127 time=0.178 ms
64 bytes from 10.10.40.228: icmp_seq=5 ttl=127 time=0.249 ms
64 bytes from 10.10.40.228: icmp_seq=6 ttl=127 time=0.224 ms
```

> 프랑크푸르트 리전에서 피어링 연결 생성.
> 

![Untitled](Public%20Cloud%2003/Untitled%2032.png)

![Untitled](Public%20Cloud%2003/Untitled%2033.png)

![Untitled](Public%20Cloud%2003/Untitled%2034.png)

![Untitled](Public%20Cloud%2003/Untitled%2035.png)

```bash
ping 10.30.40.54

PING 10.30.40.54 (10.30.40.54) 56(84) bytes of data.
64 bytes from 10.30.40.54: icmp_seq=1 ttl=127 time=226 ms
64 bytes from 10.30.40.54: icmp_seq=2 ttl=127 time=226 ms
64 bytes from 10.30.40.54: icmp_seq=3 ttl=127 time=226 ms
64 bytes from 10.30.40.54: icmp_seq=4 ttl=127 time=226 ms
64 bytes from 10.30.40.54: icmp_seq=5 ttl=127 time=226 ms
^C
--- 10.30.40.54 ping statistics ---
6 packets transmitted, 5 received, 16.6667% packet loss, time 5000ms
rtt min/avg/max/mdev = 225.984/225.997/226.024/0.014 ms
```

### Site to Site VPN

- Virtual Private Gateway
    - 암호화하여 사용.전용 회선같이 1:1로 연결한 것 같은 효과.
    - 대역폭 1GB 정도. 전용회선은 50GB.
    - 암호화와 복호화로 인한 연산이 추가되어 더 많은 시간 소요.

### Transit Gateway

- 서로 다른 리전에 있는 것과도 통신이 되는데 이 때 Transit Peering을 사용해야 함.
- 일정 네트워크 규모 이상이 되면 Transit Gateway가 VPC Peering보다 더 쌈.

### Transit Gateway&VPN 구성 실습

<aside>
✅ Virgina Region의 VPC를 On premise Data Center로 가정.
기존의 피어링 연결 삭제 후 하기

</aside>

1. Transit Gateway Subnet 생성

![Untitled](Public%20Cloud%2003/Untitled%2036.png)

![Untitled](Public%20Cloud%2003/Untitled%2037.png)

1. 서울 리전 간의 Transit Gateway 생성

![Untitled](Public%20Cloud%2003/Untitled%2038.png)

1. Transit Gateway attach 생성

![Untitled](Public%20Cloud%2003/Untitled%2039.png)

![Untitled](Public%20Cloud%2003/Untitled%2040.png)

1. Transit Gateway Routing Table 설정

![Untitled](Public%20Cloud%2003/Untitled%2041.png)

1. lab-edu-rtb-pri-01 routing table 수정

![Untitled](Public%20Cloud%2003/Untitled%2042.png)

1. lab-edu-rtb-2nd-pri-01 routing table 수정

![Untitled](Public%20Cloud%2003/Untitled%2043.png)

1. lab-edu-ec2-network-2nd-ap로 ping

```bash
ping 10.10.40.228

PING 10.10.40.228 (10.10.40.228) 56(84) bytes of data.
64 bytes from 10.10.40.228: icmp_seq=1 ttl=126 time=0.899 ms
64 bytes from 10.10.40.228: icmp_seq=2 ttl=126 time=0.743 ms
64 bytes from 10.10.40.228: icmp_seq=3 ttl=126 time=0.655 ms
64 bytes from 10.10.40.228: icmp_seq=4 ttl=126 time=0.659 ms
64 bytes from 10.10.40.228: icmp_seq=5 ttl=126 time=0.644 ms
^C
--- 10.10.40.228 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4189ms
rtt min/avg/max/mdev = 0.644/0.720/0.899/0.096 ms
```

> 서울↔프랑크푸르트 Transit Gateway 생성
> 
1. 프랑크푸르트에 Transit Gateway 생성

![Untitled](Public%20Cloud%2003/Untitled%2044.png)

1. 프랑크푸르트 리전 Transit Gateway Attach 생성

![Untitled](Public%20Cloud%2003/Untitled%2045.png)

1. 프랑크푸르트와 서울 리전 간의 Transit Gateway Attach 생성

![Untitled](Public%20Cloud%2003/Untitled%2046.png)

1. 프랑크푸르트 리전에서 보낸 요청을 서울 리전 Gateway연결 탭에서 수락.

![Untitled](Public%20Cloud%2003/Untitled%2047.png)

1. 프랑크푸르트 lab-edu-tgw-att-peering-eu에 정적 경로 생성

![Untitled](Public%20Cloud%2003/Untitled%2048.png)

![Untitled](Public%20Cloud%2003/Untitled%2049.png)

1. 서울 lab-edu-tgw-att-peering-eu에 정적 경로 생성

![Untitled](Public%20Cloud%2003/Untitled%2050.png)

1. vpc routing table 수정
- 서울 리전 lab-edu-rtb-pri-01에서 10.30.0.0/16 transit gateway(lab-edu-tgw-att-ap01) 추가
- 프랑크푸르트 리전 lab-edu-rtb-eu-pri에서 10.0.0.0/16 transit gateway(lab-edu-tgw-att-eu) 추가
1. Network 통신 테스트

```bash
ping 10.30.40.54 -c 5

PING 10.30.40.54 (10.30.40.54) 56(84) bytes of data.
64 bytes from 10.30.40.54: icmp_seq=1 ttl=124 time=230 ms
64 bytes from 10.30.40.54: icmp_seq=2 ttl=124 time=228 ms
64 bytes from 10.30.40.54: icmp_seq=3 ttl=124 time=228 ms
64 bytes from 10.30.40.54: icmp_seq=4 ttl=124 time=228 ms
64 bytes from 10.30.40.54: icmp_seq=5 ttl=124 time=228 ms

--- 10.30.40.54 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4005ms
rtt min/avg/max/mdev = 228.391/228.842/230.493/0.825 ms
```

> 서울 리전↔버지니아 리전 Transit Gateway&Site to Site VPN 연동
> 
1. 서울 리전 Custom Gateway 생성

![Untitled](Public%20Cloud%2003/Untitled%2051.png)

1. 서울 리전 Site to Site VPN 리소스 생성

![Untitled](Public%20Cloud%2003/Untitled%2052.png)

![Untitled](Public%20Cloud%2003/Untitled%2053.png)

![Untitled](Public%20Cloud%2003/Untitled%2054.png)

1. lab-edu-s2vpn-ap 구성 다운로드

![Untitled](Public%20Cloud%2003/Untitled%2055.png)

1. 버지니아 session manager에 openswan 서버 설정
- openswan 먼저 설치한 후 설정.
- aws.conf 파일 설정

```bash
sudo su -

vim /etc/sysctl.conf

# 파일에 작성할 내용
net.ipv4.ip_forward = 1  
net.ipv4.conf.default.rp_filter = 0   
net.ipv4.conf.default.accept_source_route = 0
```

```bash
vim /etc/ipsec.d/aws.conf
```

![letid → openswan public ip
right → seoul s2s vpn 외부 주소](Public%20Cloud%2003/Untitled%2056.png)

letid → openswan public ip
right → seoul s2s vpn 외부 주소

1. aws.secrets 파일 설정

```bash
vim /etc/ipsec.d/aws.ecrets

# 파일에 작성할 내용.
52.204.246.193 3.39.19.169: PSK "cloudwave"
```

1. openswan 재시작

```bash
systemctl restart network

systemctl restart ipsec

systemctl status ipsec
```

![Untitled](Public%20Cloud%2003/Untitled%2057.png)

1. 서울 리전 lab-edu-tgwrtb-ap transit gateway routing table 수정

![Untitled](Public%20Cloud%2003/Untitled%2058.png)

1. 서울리전 lab-edu-rtb-pri-01 Routing Table 수정.

![Untitled](Public%20Cloud%2003/Untitled%2059.png)

1. 버지니아 리전 lab-edu-rtb-us-pri Routing Table 수정.

![Untitled](Public%20Cloud%2003/Untitled%2060.png)

1. Network 통신 테스트

```bash
ping 10.20.40.212 -c 5

PING 10.20.40.212 (10.20.40.212) 56(84) bytes of data.
64 bytes from 10.20.40.212: icmp_seq=1 ttl=125 time=186 ms
64 bytes from 10.20.40.212: icmp_seq=2 ttl=125 time=183 ms
64 bytes from 10.20.40.212: icmp_seq=3 ttl=125 time=183 ms
64 bytes from 10.20.40.212: icmp_seq=4 ttl=125 time=183 ms
64 bytes from 10.20.40.212: icmp_seq=5 ttl=125 time=184 ms
```

<aside>
✅ openswan은 vpn opensource tool

</aside>

### VPC Endpoint

- vpc 내부에 생성하지 않은 리소스와 통신하기 위해서 natgateway를 설정하거나 internet gateway를 설정.
- vpc안에 있는 endpoint로 가면서 외부망으로 트래픽을 보내지 않아 안정하게 사용 가능.
- Gateway Endpoint는 Security Group을 정의하지 않지만, Interface Endpoint는 Security Group을 정의함.

### VPC Endpoint 구성 실습

> network-01
> 

```bash
[ec2-user@ip-10-0-40-220 ~]$ ping 1.1.1.1 -c 3
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=46 time=3.72 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=46 time=3.09 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=46 time=3.50 ms

--- 1.1.1.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 3.093/3.434/3.715/0.257 ms
[ec2-user@ip-10-0-40-220 ~]$ aws s3 ls
2024-07-16 07:55:57 cloudwave-tf-admin17
2024-07-23 02:55:55 codepipeline-ap-northeast-2-83740671875
2024-07-23 05:12:49 lab-edu-bucket-cf-repository-339712890055
2024-07-24 08:35:24 lab-edu-bucket-image-339712890055
[ec2-user@ip-10-0-40-220 ~]$ aws s3 ls s3://lab-edu-bucket-image-339712890055
2024-07-24 08:35:25      47921 cocker_spaniel.jpg
2024-07-24 08:35:25      64638 corgi.jpg
2024-07-24 08:35:25      74181 german_shepherd.jpg
2024-07-24 08:35:25      88081 golden_retriever.jpg
2024-07-24 08:35:25      41784 husky.jpg
2024-07-24 08:35:25      33828 jack_russell_terrier.jpg
2024-07-24 08:35:25      44155 jack_terrier.jpg
2024-07-24 08:35:25      65919 pug.jpg
2024-07-24 08:35:25      52571 shiba_inu.jpg
```

→ network-01은 인터넷이 가능하여 ping을 보내거나 aws 명령어를 수행했을 때 실행이 됨.

> network-02
> 

```bash
[ec2-user@ip-10-0-41-54 ~]$ aws s3 ls
^C
[ec2-user@ip-10-0-41-54 ~]$ ping 1.1.1.1 -c 1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
^C
--- 1.1.1.1 ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 0ms
```

→ 인터넷이 연결되어 있지 않아 01과 달리 통신이 되지 않음.

> network-02에서도 인터넷이 가능하도록 s3 버킷용 vpc endpoint 설정
> 
1. vpc endpoint 생성

![Untitled](Public%20Cloud%2003/Untitled%2061.png)

![Untitled](Public%20Cloud%2003/Untitled%2062.png)

1. network-02 ec2에 다시 접속하여 실행.

```bash
[ec2-user@ip-10-0-41-54 ~]$ aws s3 ls                                                                                                      
2024-07-16 07:55:57 cloudwave-tf-admin17
2024-07-23 02:55:55 codepipeline-ap-northeast-2-83740671875
2024-07-23 05:12:49 lab-edu-bucket-cf-repository-339712890055
2024-07-24 08:35:24 lab-edu-bucket-image-339712890055
[ec2-user@ip-10-0-41-54 ~]$ aws s3 ls s3://lab-edu-bucket-image-339712890055
2024-07-24 08:35:25      47921 cocker_spaniel.jpg
2024-07-24 08:35:25      64638 corgi.jpg
2024-07-24 08:35:25      74181 german_shepherd.jpg
2024-07-24 08:35:25      88081 golden_retriever.jpg
2024-07-24 08:35:25      41784 husky.jpg
2024-07-24 08:35:25      33828 jack_russell_terrier.jpg
2024-07-24 08:35:25      44155 jack_terrier.jpg
2024-07-24 08:35:25      65919 pug.jpg
2024-07-24 08:35:25      52571 shiba_inu.jpg
```

→ vpc endpoint가 생기면서 통신이 가능해짐.

> SSM VPC Endpoint 생성(Interface Type)
> 

![Untitled](Public%20Cloud%2003/Untitled%2063.png)

→ network-02는 외부에서 접근이 불가능한 상태라 session manager 사용 불가.
→ session manager를 사용할 수 있도록 vpc endpoint 생성.

1. SSM VPC Endpoint용 Security Group 생성

![Untitled](Public%20Cloud%2003/Untitled%2064.png)

1. VPC Endpoint 생성
- Endpoint 생성 전 lab-edu-vpc-ap-01 VPC DNS 설정에서 DNS 호스트 이름 활성화.

![Untitled](Public%20Cloud%2003/Untitled%2065.png)

- SSM Endpoint 생성

![Untitled](Public%20Cloud%2003/Untitled%2066.png)

![Untitled](Public%20Cloud%2003/Untitled%2067.png)

- SSMessages Endpoint 생성.

![Untitled](Public%20Cloud%2003/Untitled%2068.png)

![Untitled](Public%20Cloud%2003/Untitled%2069.png)

- EC2Messages Endpoint 생성

![Untitled](Public%20Cloud%2003/Untitled%2070.png)

![Untitled](Public%20Cloud%2003/Untitled%2071.png)

1. SSM이용 Network Server 접속

![Untitled](Public%20Cloud%2003/Untitled%2072.png)

→ Endpoint를 생성함으로써 Session Manager로 인스턴스에 연결이 가능해짐.

→ Endpoint가 있더라도 외부에서 접근 불가하므로 내부에서 안전하게 접근 가능.
