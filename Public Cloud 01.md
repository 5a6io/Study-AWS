# Public Cloud 01

Files & media: streamlit-project-main.zip, %25EA%25B5%2590%25EC%259E%25AC_Public_Cloud_-_Chapter02._Cloud_Fundermental.pdf, %25EA%25B5%2590%25EC%259E%25AC_Public_Cloud_-_Chapter03._Foundation_Architecture_Configuration.pdf

## Cloud Computing Deployment Model

### Public Cloud

누구나 시스템과 서비스에 접근 가능하도록 함. 모든 것에 열려있어서 보안성

### Private Cloud

### Hybrid Cloud

민감한 데이터는 Private에 저장하고 Private과 Public 사이 네트워크로만 연결.

### Multi Cloud

## Cloud Computing Model

### On-Premiss

### Colocation

### Hosting

### IaaS

### PaaS

- Binstock →

### SaaS

사용자가 데이터만 관리. Network부터 Application까지의 서비스는 제공해줌.

## AWS Cloud

### AWS Regions

### Availability Zones

건물 또는 층 단위로 구분.

### ARN

- AWS가 서비스를 고유하게 식별하기 위해서 부여하는 네이밍 컨벤션.

### 시스템 개발 환경의 분류

Local → 노트북이나 PC같은 Local 환경에서 Code 개발.

 ⬇️

Dev → Code 단에서 테스트.

 ⬇️

Staging → Dummy Data를 넣어놓고 Production 환경과 유사한 환경에서 테스트.

 ⬇️

QA → 

 ⬇️

Production

### 사용자가 생성한 리소스의 네이밍 컨벤션

- kebab-case → amazon에서 사용.
- 회사마다 네이밍 컨벤션이 다름.
    - 시스템 개발 환경 - 시스템 명칭 - 리소스 명칭 - 시스템 역할 - a + 넘버링
                                                          ↳추가 정보

## Network Baseline Configuration

### Virtual Private Cloud

### Public & Private Subnet

### Internet Gateway

### Nat Gateway

- private에서 외부 환경으로 나갈 수는 있으나, 외부에서 private으로는 못 들어옴.

### Network Baseline 구성 실습

1. VPC 생성

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled.png)

1. Public Subnet 2개 생성.

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%201.png)

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%202.png)

1. Private Subnet 2개 생성.
2. Internet Gateway 생성.
3. Public Routing Table 설정.
4. Nat Gateway 설정.
- Public Subnet에 생성을 해줘야 함. → private subnet이 외부와 통신할 수 있도록 해줘야 하기 때문에 Nat Gateway 자체도 외우와 통신이 가능하도록 해야 함.
1. Private Routing Table 설정.

> lab-edu-rtb-pri-01
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%203.png)

> lab-edu-rtb-pri-02
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%204.png)

## Computing Resource Configuration

### Elastic Compute Cloud

- IaaS
- Amazon EC2

### Computing Service 구성 실습

root 사용자로 실행.

> vi install_python.sh
> 

```bash
#!/bin/bash
# Python 3.11 설치 스크립트
echo "#####################"
echo "Python 3.11 설치 시작"
echo "#####################"
sudo dnf install python3.11 -y
echo "##############################"
echo "Python Default Version Setting"
echo "update-alternatives 명령 실행."
echo "##############################"
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 2
echo 2 | sudo update-alternatives --config python3
echo
echo "################################"
echo "Python 3.11 PIP Module 설치 시작"
echo "################################"
python3 -m ensurepip --default-pip
echo "##############################"
echo "Python3 → Python Symbolic link"
echo "##############################"
echo "sudo ln -fs /usr/bin/python3.11 /usr/bin/python"
sudo ln -fs /usr/bin/python3.11 /usr/bin/python
echo "#######################"
echo "Python 3.11 설치 완료"
echo "설치된 Python 버전 정보"
echo "#######################"
python --version
echo "#####################################"
echo "dnf/yum에서 사용하는 Python 버전 지정"
echo "#####################################"
head -1 /usr/bin/dnf
sudo sed -i 's|#!/usr/bin/python3|#!/usr/bin/python3.9|g' /usr/bin/dnf
sudo sed -i 's|#!/usr/bin/python3|#!/usr/bin/python3.9|g' /usr/bin/yum
head -1 /usr/bin/dnf
sudo dnf --version
echo "################"
echo "PIP Upgrade 설치"
echo "################"
python3 -m pip install --upgrade pip
echo "################"
echo "Stress Tool 설치"
echo "################"
sudo dnf install stress -y
echo "##################"
echo "Git & Code Install"
echo "##################"
yum install git -y
cd /root/
git clone https://github.com/sh1517/streamlit-project.git
echo "######################"
echo "Python Package Install"
echo "######################"
cd streamlit-project
pip install -r requirements.txt
echo "##############"
echo "Upgrade AWSCLI"
echo "##############"
sh scripts/upgrade_aws_cli_v2.sh
echo "#################"
echo "Application Start"
echo "#################"
streamlit run main.py --server.port 80
```

> lab-edu-ec2-web 생성.
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%205.png)

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%206.png)

> bastion에서 web-server로 접속
> 

```bash
[ec2-user@ip-10-0-0-176 ~]$ vim lab-edu-key-ec2.pem
[ec2-user@ip-10-0-0-176 ~]$ ls
lab-edu-key-ec2.pem
[ec2-user@ip-10-0-0-176 ~]$ ls -l
total 4
-rw-r--r--. 1 ec2-user ec2-user 1679 Jul 22 05:11 lab-edu-key-ec2.pem
[ec2-user@ip-10-0-0-176 ~]$ chmod 600 lab-edu-key-ec2.pem
[ec2-user@ip-10-0-0-176 ~]$ ls -ll
total 4
-rw-------. 1 ec2-user ec2-user 1679 Jul 22 05:11 lab-edu-key-ec2.pem
[ec2-user@ip-10-0-0-176 ~]$ ssh -i lab-edu-key-ec2.pem ec2-user@10.0.40.25
The authenticity of host '10.0.40.25 (10.0.40.25)' can't be established.
ED25519 key fingerprint is SHA256:E3Oy1NaDuahPwfiTK4zJMmOXKNz0iOYQrxgXKfLZwHA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.40.25' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip-10-0-40-25 ~]$ su -
Password:
[ec2-user@ip-10-0-40-25 ~]$ sudo su -
[root@ip-10-0-40-25 ~]# netstat -ntlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1603/sshd: /usr/sbi
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      25008/python3
tcp6       0      0 :::22                   :::*                    LISTEN      1603/sshd: /usr/sbi
tcp6       0      0 :::80                   :::*                    LISTEN      25008/python3
[root@ip-10-0-40-25 ~]# ps -ef | grep streamlit
root       25008    1645  0 05:09 ?        00:00:01 /usr/bin/python3 /usr/local/bin/streamlit run main.py --server.port 80
root       25377   25303  0 05:16 pts/1    00:00:00 grep --color=auto streamlit
```

> Workspace Settings
> 

```bash
# work space 폴더
mkdir cloud-wave-workspace
cd cloud-wave-workspace

# 특정 파일만 git에서 pull
git config --global init.defaultBranch main
git init
git remote add origin https://github.com/sh1517/streamlit-project.git
git config core.sparseCheckout true
echo "images/" >> .git/info/sparse-checkout
echo "scripts/" >> .git/info/sparse-checkout
echo "support_files/" >> .git/info/sparse-checkout
echo "serverless_code/" >> .git/info/sparse-checkout
git pull origin main
```

> cloud9 생성
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%207.png)

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%208.png)

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%209.png)

> cloud9에 EC2 pem key를 업로드하여 bastion과 web server 접속 테스트
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%2010.png)

→ web-server는 cloud9에서 ssh로 접속이 가능하나, bastion은 접속이 불가능.

→ bastion의 인바운드 규칙에 내 퍼블릭 IP에서만 접속이 가능하도록 하였기 때문에 접속 불가능.

> bastion 인바운드 규칙에 10.0.0.6/16을 추가하여 접속이 가능하게 됨.
> 

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%2011.png)

### Elastic Load Balancer

- AWS ELB
- 클라이언트의 서비스 요청 트래픽을 다수의 서버로 분산시켜 주는 서비스.

### Load Balancer Service 구성 실습

1. 보안 그룹 설정
2. Load Balancer Target Group 생성
3. Load Balancer 생성

![Untitled](Public%20Cloud%2001%209d2859d3c9b04d6da724f490e84da6fa/Untitled%2012.png)

### Relational Database

- Amazon RDS
- Multi AZ → 이중화.
- Read Replica → 분산 처리.

### Database Service 구성 실습

Database에도 서브넷 생성 → 내부에서 내보내지 못하고 외부에서 접근하지 못하도록 하기 위해서.

cloud9에서 RDS로 접근할 수 있도록 하려면?

보안그룹의 인바운드 규칙을 설정하거나 네트워크 ACL을 이용.

1. 서브넷 생성
2. 라우팅 테이블 생성
3. DB 서브넷 그룹 생성
4. 보안 그룹 생성
- 보안 그룹 설정 시 IP가 아닌 보안 그룹으로도 설정 가능.
    - 보안 그룹을 갖고 있는 인스턴스나 RDS에도 해당 보안 그룹이 적용될 수 있음.
1. RDS 생성
2. PostgrSQL 설치

```bash
cd ~/environment/cloud-wave-workspace/scripts/
sh install_postgresql_in_AmazonLinux2023.sh
```

1. PostgreSQL 접속

`psql -U postgres` 만 적으면 cloud9에 설치된 postgres에 접속 `-h <End Point>`를 입력해야지 해당 주소의 postgresql로 접속.

> DB 생성.
> 

```bash
postgres=> create database trip_advisor;
CREATE DATABASE
postgres=> create user "user" with password 'qwer1234';
CREATE ROLE
postgres=> grant all privileges on database trip_advisor to "user";
GRANT
postgres=> alter database trip_advisor owner to "user";
ALTER DATABASE
```

## Security Resource Configuration

### Security Group

- 리소스에 접근 허용할 트래픽 정보를 관리하는 가상의 방화벽 서비스
- 허용 트래픽에 대한 규칙만 정의(Any deny)
- 상태 저장 방식 방화벽
    - 인바운드로 허용한 규칙이 있으면 아웃바운드가 없어도 응답 트래픽은 가능.
- Security Group Quotas
    - IP(Amazon에서는 ENI)에 Security Group이 적용.

### Network ACL

- 서브넷에 접근 허용할 트래픽 정보를 관리하는 가상의 방화벽 서비스
- 허용/차단 트래픽에 대한 규칙 모두 정의
- 상태 비저장 방식 방화벽
    - 인바운드로는 규칙을 적용하고 아웃바운드로는 허용 규칙이 없으면 트래픽이 거부됨.
    - 범위로 지정해야 함.
- Network ACL Quotas
    - 금융권은 NACL을 주로 사용.