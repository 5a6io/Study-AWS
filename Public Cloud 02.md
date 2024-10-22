# Public Cloud 02

Files & media: %25EA%25B5%2590%25EC%259E%25AC_Public_Cloud_-_Chapter04._Architecting_on_Cloud_-_1._Networke.pdf

## Review

### IaaS, SaaS, PaaS

‘사업자가 서비스를 어디까지 제공하는가’에 따라 다름.

### 시스템 개발 환경

대부분 Dev, Prod 정도만 테스트

### Subnet

## Security Resource Configuration

### Identity Access Management

- IAM

## IAM 서비스 구성 실습

### IAM User 생성

1. IAM User AdministratorAccess 권한 할당

> 사용자 새로 생성.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled.png)

> Administrator Access 권한 부여

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%201.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%202.png)

> https://339712890055.signin.aws.amazon.com/console로 접속

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%203.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%204.png)

→ Administrator Access 권한이 있어서 인스턴스를 관리할 수 있음.

> 권한 삭제 후 EC2 콘솔 재접속.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%205.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%206.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%207.png)

→ 권한이 부여되지 않아 오류가 발생.

1. Access Key와 Secret Key를 이용해서 접속.

> ReadOnlyAccess 권한을 부여.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%208.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%209.png)

> Access Key에 대한 정보를 Bastion 서버에 입력.

```bash
[ec2-user@ip-10-0-0-176 ~]$ sudo su -
Last login: Mon Jul 22 03:38:15 UTC 2024 on pts/1
[root@ip-10-0-0-176 ~]# aws configure
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: ap-northeast-2
Default output format [None]: json
[root@ip-10-0-0-176 ~]# aws sts get-caller-identity
{
    "UserId": "AIDAU6GDXKTD3F3SC3NMX",
    "Account": "339712890055",
    "Arn": "arn:aws:iam::339712890055:user/lab-edu-iam-user-01"
}
[root@ip-10-0-0-176 ~]# cd streamlit-project/
[root@ip-10-0-0-176 streamlit-project]# streamlit run main.py --server.port 80 &
[1] 58815
[root@ip-10-0-0-176 streamlit-project]#
Collecting usage statistics. To deactivate, set browser.gatherUsageStats to False.

  You can now view your Streamlit app in your browser.

  Network URL: http://10.0.0.176:80
  External URL: http://43.203.233.160:80
```

> configure로 user에 대한 정보를 입력한 후 80번 포트를 열고 43.203.233.160로 접속

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2010.png)

### IAM role 생성

> lab-edu-role-ec2 새 역할 생성.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2011.png)

> Administrator Access 권한 설정.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2012.png)

> lab-edu-role-ec2를 lab-edu-ec2-web-server에 부여

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2013.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2014.png)

> 로드밸런서의 dns를 통해 lab-edu-alb-web-1086277303.ap-northeast-2.elb.amazonaws.com로 접속하여 web-server 확인

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2015.png)

## Dev Tools Resource Configuration

AWS CI/CD

### AWS Code Series

- AWS CodeCommit
  - AWS에서 제공하는 Private Git Repository
  - 보안상 유리.
- AWS CodeBuild
- AWS CodeDeploy
- AWS CodePipeline

## CodeSeries 서비스 구성 실습

### CodeCommit 생성

> CodeCommit Repository 생성 → Local에 실습 코드 다운로드

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2016.png)

[GitHub - sh1517/streamlit-project](https://github.com/sh1517/streamlit-project/tree/main)

> ssh key 생성

```bash
C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\KDP/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\KDP/.ssh/id_rsa
Your public key has been saved in C:\Users\KDP/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:3GrgCUv1w5vGEpBY2dGQmH2/z3a8oNEyDmJebqFfJfg kdp@KHP18
The key's randomart image is:
+---[RSA 3072]----+
|    .*o=         |
|   o+.+ o        |
|  . o .. .       |
|     o + o.      |
|    o o S o..    |
|   . + =.*.+     |
|    . *.X.Eoo.   |
|     o.B.+ =+.o  |
|      .oo o. ... |
+----[SHA256]-----+
```

> lab-edu-iam-user-01 사용자에게 AWSCommitFullAccess 권한 부여

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2017.png)

> 이전에 만든 ssh의 publice key에 대한 정보 업로드

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2018.png)

> Config에 user에 대한 정보 기록

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2019.png)

> Config의 정보를 이용하여 ssh push 테스트

```bash
C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>ssh git-codecommit.us-east-2.amazonaws.com
You have successfully authenticated over SSH. You can use Git to interact with AWS CodeCommit. Interactive shells are not supported.Connection to git-codecommit.us-east-2.amazonaws.com closed by remote host.
Connection to git-codecommit.us-east-2.amazonaws.com closed. ➡️ ssh를 이용하여 연결 테스트 후 연결 성공.

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git init
Initialized empty Git repository in C:/Users/KDP/Desktop/streamlit-project-main/streamlit-project-main/.git/

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git branch -M main

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git remote add origin ssh://git-codecommit.ap-northeast-2.amazonaws.com/v1/repos/lab-edu-code-streamlit 

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git add . 

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git commit
 -m "first commit"
[main (root-commit) 9a09af8] first commit
 90 files changed, 5266 insertions(+)

C:\Users\KDP\Desktop\streamlit-project-main\streamlit-project-main>git push origin main
The authenticity of host 'git-codecommit.ap-northeast-2.amazonaws.com (52.95.194.93)' can't be established.
RSA key fingerprint is SHA256:eegAPQrWY9YsYo9ZHIKOmxetfXBHzAZd8Eya53Qcwko.   
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'git-codecommit.ap-northeast-2.amazonaws.com' (RSA) to the list of known hosts.
Enumerating objects: 110, done.
Counting objects: 100% (110/110), done.
Delta compression using up to 16 threads
Compressing objects: 100% (103/103), done.
Writing objects: 100% (110/110), 573.26 KiB | 2.36 MiB/s, done.
Total 110 (delta 13), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Validating objects: 100%
To ssh://git-codecommit.ap-northeast-2.amazonaws.com/v1/repos/lab-edu-code-streamlit
 * [new branch]      main -> main
```

→ ssh를 이용하여 AWS의 생성한 repository에 파일 업로드

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2020.png)

### CodeDeploy 생성

> web-server로 접속 후 CodeDeploy Agent 설치

```bash
# 관련 패키지 다운로드
sudo yum update -y && sudo yum install ruby -y && sudo yum install wget -y

#CodeDeploy Agent 설치 프로그램 다운로드
wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install

# 설치 파일 실행 권한 부여
chmod +x ./install

# 설치 파일 이용 최신 버전 설치
sudo ./install auto

# CodeDeploy Agent 서비스 실행 및 상태 확인 명령
systemctl status codedeploy-agent   # sudo service codedeploy-agent status
systemctl start codedeploy-agent    # sudo service codedeploy-agent start
```

> CodeDeploy IAM Role 생성

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2021.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2022.png)

> CodeDeploy Application & Deploy 생성
> 애플리케이션 생성 후 배포그룹 생

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2023.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2024.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2025.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2026.png)

> 변경 후 오류 발생.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2027.png)

→ codedeploy-agent를 삭제하고 재시도

```bash
sudo yum erase codedeploy-agent
cd /opt
rm -rf codedeploy-agent
./install auto
systemctl stop codedeploy-agent
systemctl start codedeploy-agent
```

Deploy 실패한 작업 재시도 후 로드밸런서 dns로 web-server 접속.

start stress test

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2028.png)

autoscaling

## Management Resource Configuration

### CloudFormation

## CloudFormation 서비스 구성 실습

### Network Baseline 추가

1. lab-edu-iam-user-01 추가
2. Administrator Access 권한 추가
3. CloudFormation YAML 파일 S3업로드

```bash
# cmd에서 실행
# Account ID 값을 확인하고 환경변수 설정
aws sts get-caller-identity --query Account --output text
set ACCOUNT_ID=339712890055
echo %ACCOUNT_ID%

#S3 bucket 생성
set BUCKET_NAME=lab-edu-bucket-cf-repository-%ACCOUNT_ID%
echo %BUCKET_NAME%
aws s3 mb s3://%BUCKET_NAME%

# s3에 데이터 업로드
set FILE_PATH=support_files\infra_as_a_code\ap-northeast-2
set FILE_NAME=01. vpc_resource.yaml
set OBJ_NAME=network-baseline.yaml
aws s3 cp "%FILE_PATH%\%FILE_NAME%" s3://%BUCKET_NAME%/%OBJ_NAME%
```

1. YAML Template 이용 VPC 생성
- YAML Template 파일 구성 확인

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2029.png)

- 객체 URL 정보 확인 → URL 정보 복사

```bash
set OBJ_URL=https://%BUCKET_NAME%.s3.amazonaws.com/%OBJ_NAME%
echo %OBJ_URL%
```

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2030.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2031.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2032.png)

> Parameter Section을 활용해 Public Subnet 생성

- s3에 데이터 업로드

```bash
set FILE_NAME=02. subnet_resource.yaml
aws s3 cp "%FILE_PATH%\%FILE_NAME%" s3://%BUCKET_NAME%/%OBJ_NAME%

# 객체 정보 확인 후 URL 정보 복사
echo %OBJ_URL%
```

- CloudFormation 메인 콘솔 화면에서 스택 리소스 탭 → lab-edu-cf-network-baseline-ap 스택 선택 후 업데이트

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2033.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2034.png)

1. 01.vpc_resource.yaml로 적용되어있던 스택 세부 정보를 02.subnet_resource.yaml로 업데이트

```yaml
Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.10.0.0/16
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: lab-edu-vpc-ap-02
```

```yaml
Parameters:
  VpcCIDR:
    Description: CIDR Block for VPC
    Type: String
    Default: 10.10.0.0/16
  PublicSubnet01CIDR:
    Description: CIDR Block for Publicsubnet01
    Type: String
    Default: 10.10.0.0/24
  PublicSubnet02CIDR:
    Description: CIDR Block for Publicsubnet01
    Type: String
    Default: 10.10.1.0/24

  VpcId:
    Description: ID for VPC
    Type: String
    Default: lab-edu-vpc-ap-02
  PublicSubnet01Id:
    Description: ID for PublicSubnet01
    Type: String
    Default: lab-edu-sub-2nd-pub-01
  PublicSubnet02Id:
    Description: ID for PublicSubnet02
    Type: String
    Default: lab-edu-sub-2nd-pub-02

  AvailabilityZoneSubnet01:
    Description: AvailabilityZone for every First subnet
    Type: AWS::EC2::AvailabilityZone::Name
  AvailabilityZoneSubnet02:
    Description: AvailabilityZone for every Second subnet
    Type: AWS::EC2::AvailabilityZone::Name

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: !Ref VpcCIDR
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: !Ref VpcId
  PublicSubnet01:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PublicSubnet01CIDR
      AvailabilityZone: !Ref AvailabilityZoneSubnet01
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: !Ref PublicSubnet01Id
  PublicSubnet02:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: !Ref PublicSubnet02CIDR
      AvailabilityZone: !Ref AvailabilityZoneSubnet02
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: !Ref PublicSubnet02Id
```

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2035.png)

→ Public Subnet 구성 추가

1. Output Section 활용

```bash
# s3에 데이터 업로드 후 URL 복사
set FILE_NAME=03. resource_output.yaml
aws s3 cp "%FILE_PATH%\%FILE_NAME%" s3://%BUCKET_NAME%/%OBJ_NAME%
echo %OBJ_URL%
```

- 기존 템플릿을 현재 올린 템플릿으로 교체

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2036.png)

1. resource_output.yaml 적용.

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2037.png)

> Label로 그룹이름 설정.

```yaml
Metadata: 
  AWS::CloudFormation::Interface: 
    ParameterGroups: 
      - 
        Label: 
          default: "Network Configuration"
        Parameters: 
          - VpcId
          - VpcCIDR
          - PublicSubnet01Id
          - PublicSubnet01CIDR
          - PublicSubnet02Id
          - PublicSubnet02CIDR
          - PrivateSubnet01Id
          - PrivateSubnet01CIDR
          - PrivateSubnet02Id
          - PrivateSubnet02CIDR
          - DatabaseSubnet01Id
          - DatabaseSubnet01CIDR
          - DatabaseSubnet02Id
          - DatabaseSubnet02CIDR
          - TransitGWSubnet01Id
          - TransitGWSubnet01CIDR
          - TransitGWSubnet02Id
          - TransitGWSubnet02CIDR

          - PublicRoutingTableId
          - PrivateRoutingTable01Id 
          - PrivateRoutingTable02Id 
          - DatabaseRoutingTableId
          - TransitGWRoutingTableId

      - 
        Label: 
          default: "Computing Resource Configuration"
        Parameters: 
          - EC2OS
          - EC2InstanceNameNetwork
          - EC2InstanceType
          - EC2InstanceRoleName
          - EC2SecurityGroupNetwork
          - VPCEndpointSecurityGroupName

          - AvailabilityZoneSubnet01
          - AvailabilityZoneSubnet02
```

1. Metadata Section의 ParameterGroups 활용

```bash
# 
set FILE_NAME=04. metadata_parameter_groups.yaml
aws s3 cp "%FILE_PATH%\%FILE_NAME%" s3://%BUCKET_NAME%/%OBJ_NAME%
echo %OBJ_URL%
```

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2038.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2039.png)

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2040.png)

```bash
# IaC 코드 파일이 있는 폴더로 이동
cd ~/environment/cloud-wave-workspace/support_files/infra_as_a_code/

# 서울 리전, 버지니아 리전 YAML 파일 비교
diff -y ap-northeast-2/network_baseline.yaml us-east-1/network_baseline.yaml

# 서울 리전, 프랑크푸르트 리전 YAML 파일 비교
diff -y ap-northeast-2/network_baseline.yaml eu-central-1/network_baseline.yaml
```

1. 버지니아 리전 network_baseline.yaml 배포

```bash
set FILE_PATH=support_files/infra_as_a_code/us-east-1

set FILE_NAME=network_baseline.yaml

set STACK_NAME=lab-edu-cf-network-baseline-us

set REGION=us-east-1

aws cloudformation create-stack --stack-name %STACK_NAME% ^
--template-body file://%FILE_PATH%/%FILE_NAME% ^
--capabilities CAPABILITY_NAMED_IAM ^
--region %REGION% ^
--parameters ^
ParameterKey=AvailabilityZoneSubnet01,ParameterValue=%REGION%a ^
ParameterKey=AvailabilityZoneSubnet02,ParameterValue=%REGION%c
```

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2041.png)

1. 프랑크프루트 리전 network_baseline.yaml 배포

```bash
set FILE_PATH=support_files/infra_as_a_code/eu-central-1

set REGION=eu-central-1

set STACK_NAME=lab-edu-cf-network-baseline-eu

aws cloudformation create-stack --stack-name %STACK_NAME% ^
--template-body file://%FILE_PATH%/%FILE_NAME% ^
--capabilities CAPABILITY_NAMED_IAM ^
--region %REGION% ^
--parameters ^
ParameterKey=AvailabilityZoneSubnet01,ParameterValue=%REGION%a ^
ParameterKey=AvailabilityZoneSubnet02,ParameterValue=%REGION%c
```

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2042.png)

## Network

### Network Design Fundamentals

1. 네트워크의 세 가지 설계 포인트
- SEGMENTATION → CIDR은 충돌이 나면 안 됨.
- CONNECTIVITY → 필요한 것들은 열어야 함.전용선을 써서 열어야 하는 경우 어떻게 해야 하는지 고려해야 함.
- SECURITY →
1. RFC 1918 - Standard Private IP Address
- 내부 네트워크망 구성은 Private Ip Address Range 내 할당 → Public IP로 할당할 경우 충돌이 발생할 수 있음.
1. Usable IP Address Range in AWS VPC per CIDR

### AWS Cloud Network Service

1. Amazon Route 53
- DNS 서비스 제공
- 유일하게 100% SLA 제공 → 장애가 발생하면 환불해줌.
- Public Hosted Zone → Domain을 입력하면 Route 53을 거쳐 IP를 받고 서버로 넘어감.
- Private Hosted Zone → Domain을 Route 53을 거쳐 람다 같은 함수를 이용하여 실제 ip로 변경(?)
- Routing Policy
  - Simple → 여러 개를 할당한다면 모두 다 주고 클라이언트에서 1개를 선택하여 사용.
  - Weighted → 10번 요청하면 설정한 가중치만큼 전달됨. 가충지가 0이라면 균등한 가중치를 적용.
  - Latency → 요청을 보냈을 때 더 빠르게 응답하는 서버에 요청을 넘김. 다중 리전 환경에서는 클라이언트와 가장 근접한 리소스로 연결될 확률이 높음.
  - Failover → 기본으로 구성되어있는 서버로 넘기다가 죽으면 보조 서버로 넘김. 기본 서버가 healthy 상태가 되면 다시 기본 서버로 넘김.
  - Geolocation → 클라이언트가 요청을 보내는 위치를 기반으로 가장 가까운 리전의 서버의 IP를 반환.
    
                EDNS:서울 리전에서 미국 리전을 바라보고 있다면 바라보고 있는 미국 리전 위치를 기반으로 가장 가까운 리전 서버의 IP를 반환.

## Route 53 Policy 구성 실습

![Untitled](Public%20Cloud%2002%20776b222c6bdc4abeb80d7f81b40fc70f/Untitled%2043.png)