# Public Cloud 04

## AWS Cloud Network Service

### CloudFront

- 빠르게 클라이언트에게 전달해주기 위한 서비스.
- 메모리 상에 올려놓은 데이터를 가져와 서버 뒷단까지 가지 않고 빠르게 받을 수 있음.
- 메모리 상에 올려놓지 않은 경우 → Cache Miss
- Cache Miss가 발생하면 먼저 원본 데이터까지 갖다가 메모리에 올려놓고 클라이언트에서 전송.
- Edge Location
- 트래픽이 분산되는 효과를 가져올 수 있음. → Anti DDoS 같은 효과를 얻을 수 있음.
- S3는 HTTPS를 설정할 수 없으므로

## Computing System

### Amazon EC2

- Naming Rule
    - Instatance Type - Instance Generation - Attributes - Size
                                                                                  ↳CPU 종류
- Family & Type
- Generation
    - 이전 세대보다 비용이 낮아지거나 동일 가격 대비 성능 향상.
- Attribute & Size
- Burstable performance instances
    - T타입 → 개발이나 테스트 환경에 적합. Prod은 M타입을 쓰는 것이 좋음.
    - 기준 성능 이하인 경우에는 크레딧이 계속 쌓임. Burstable Zone에 들어가는 경우 크레딧을 소진.
- EC2 Instatnce Tenancy
    - 전용 인스턴스 → 물리 서버 하나에 계속 배치됨. 물리 서버가 장애가 나면 기존 물리 서버에 다시 배치된다고 보장할 수 없음.
- Autoscaling
    - 특정 조건에 도달하면 서버 수량 자체를 늘리거나 크기를 키움.
    - Scaling In & Out - 서버 자체의 수량을 늘리거나 줄임.
    - Scaling Up & Down - 서버의 성능을 향상시키거나 낮춤.
    - 스펙은 일정 수준 맞춰놓고  서비스가 증가할수록 서버 개수를 늘림.

### AMI 활용 Web 서버 복제 실습

1. 인스턴스 생성

> Quick Start가 아닌 OpenVPN 검색
> 

![Untitled](Public%20Cloud%2004/Untitled.png)

![Untitled](Public%20Cloud%2004/Untitled%201.png)

![Untitled](Public%20Cloud%2004/Untitled%202.png)

![Untitled](Public%20Cloud%2004/Untitled%203.png)

![Untitled](Public%20Cloud%2004/Untitled%204.png)

![Untitled](Public%20Cloud%2004/Untitled%205.png)

![Untitled](Public%20Cloud%2004/Untitled%206.png)

> admin으로 로그인 후
> 

![Untitled](Public%20Cloud%2004/Untitled%207.png)

![Untitled](Public%20Cloud%2004/Untitled%208.png)

![Untitled](Public%20Cloud%2004/Untitled%209.png)

![Untitled](Public%20Cloud%2004/Untitled%2010.png)

### Elastic Load Balancer

- webserver → 기본적으로 HTTPS로  구성.
- health check해서 traffic을 분산해줌.
- AWS ELB 종류
    - Application Load Balancer
    - Network Load Balancer → Elastic IP 적용 가능. 즉 고정적인 public ip 적용 가능.
    - Gateway Load Balancer → L3 계층 지원. IP protocol.
    - Classic Load Balancer → 1세대 LB. 사용 권장X. → 마이그레이션하는 것이 좋지만 불가능한 경우도 있음.
- Auto Scaling 서비스와의 연동
    - ELB 안에 속해 있는 EC2의 개수가 늘어나거나 줄어드는 것.

### Load Balancer와 ACM, Auto-Scaling 서비스 연동

1. 인증서 생성

![Untitled](Public%20Cloud%2004/Untitled%2011.png)

![Untitled](Public%20Cloud%2004/Untitled%2012.png)

![Untitled](Public%20Cloud%2004/Untitled%2013.png)

![Untitled](Public%20Cloud%2004/Untitled%2014.png)

→ 맨 밑 6줄 삭제. 인스턴스 재시작 시에도 streamlit이 재시작할 수 있도록 하기 위함.

→ EC2에서 파일을 변경하였으므로 이미지 재생성.

![Untitled](Public%20Cloud%2004/Untitled%2015.png)

![Untitled](Public%20Cloud%2004/Untitled%2016.png)

![Untitled](Public%20Cloud%2004/Untitled%2017.png)

![Untitled](Public%20Cloud%2004/Untitled%2018.png)

> Load Balancer Listener 등록
> 

![Untitled](Public%20Cloud%2004/Untitled%2019.png)

→ 인증서가 들어가 경고문고가 뜨지 않음.

> 리스너 편집
> 

![Untitled](Public%20Cloud%2004/Untitled%2020.png)

![Untitled](Public%20Cloud%2004/Untitled%2021.png)

![Untitled](Public%20Cloud%2004/836bf5f5-7e38-46b2-9c2f-e679e45c68b2.png)

![Untitled](Public%20Cloud%2004/afbc5a72-0f30-4b7f-9a69-e297530b8556.png)

![Untitled](Public%20Cloud%2004/Untitled%2022.png)

![Untitled](Public%20Cloud%2004/Untitled%2023.png)

![Untitled](Public%20Cloud%2004/Untitled%2024.png)

![Untitled](Public%20Cloud%2004/Untitled%2025.png)

![Untitled](Public%20Cloud%2004/Untitled%2026.png)

![Untitled](Public%20Cloud%2004/Untitled%2027.png)

![Untitled](Public%20Cloud%2004/Untitled%2028.png)

![Untitled](Public%20Cloud%2004/Untitled%2029.png)

![Untitled](Public%20Cloud%2004/Untitled%2030.png)

![Untitled](Public%20Cloud%2004/Untitled%2031.png)

### Lambda

- serverless → 내가 관리할 필요가 없다.
- 평소에는 꺼져 있다가 event가 발생할 때 수행.
- 10GB 이상을 쓰려면 EFS와 같은 것이 필요함.

## Storage System

### AWS Storage Porfolio

- 저장, 보호, 전송 세 가지로 서비스 영역 구분.
- Block Storage
    - Amazon EBS
    - Object와 File보다는 빠름.
    - File Storage보다는 덜 비쌈.
- FIle Storage
    - EFS도 마운트 지원.
    - 파일 단위로 저장.
    - FSx는 리눅스 윈도우에 다 사용 가능.
    - EFS는 윈도우 밖에 안 됨.
- Object Storage
    - HTTP가 가능한 어디서든 접근 가능.

### EBS(Elastic Block Store)

- 네트워크 드라이브
- 네트워크 너머에 있는
- Instance Store
    - 대용량 데이터나
- Snapshot 기능 제공.
- 암호화 지원
- io1, 2타입을 사용하면 여러 인스턴스에 연결 가능.
- LVM으로 디스크 하나를 ㄷ
- 같은 가용영역에 있는 인스턴스끼리만 연결 가능.
- 스냅샷을 이용해 다른 가용영역으로 본제본을 만들어 사용 가능.
- gp3, gp2를 가장 많이 사용.
- Volume Encryption → 네트워크 구간을 탈 때는 . 이미 생성한 것에는 암호화 설정 불가능.

### EBS Volume 연결 해제 및 재연결(손상된 SSH authorized key 복구 시나리오)

bastion

/dev/sdf

### EBS Volume 생성 및 추가 연결

![Untitled](Public%20Cloud%2004/Untitled%2032.png)

![Untitled](Public%20Cloud%2004/Untitled%2033.png)

![Untitled](Public%20Cloud%2004/Untitled%2034.png)

```bash
AWSReservedSSO_AdministratorAccess_c3f9c44a58eb4f25:~/environment $ ssh network-01

# network-01 접속 후 연결된 정보 확인
[ec2-user@ip-10-0-40-220 ~]$ lsblk
NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
nvme0n1       259:0    0   8G  0 disk 
├─nvme0n1p1   259:1    0   8G  0 part /
├─nvme0n1p127 259:2    0   1M  0 part 
└─nvme0n1p128 259:3    0  10M  0 part /boot/efi
nvme1n1       259:4    0  10G  0 disk 

#볼륨 파일 시스템 포맷
[ec2-user@ip-10-0-40-220 ~]$ sudo mkfs.xfs /dev/nvme1n1
meta-data=/dev/nvme1n1           isize=512    agcount=16, agsize=163840 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1
data     =                       bsize=4096   blocks=2621440, imaxpct=25
         =                       sunit=1      swidth=1 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=512   sunit=1 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[ec2-user@ip-10-0-40-220 ~]$ df -Th
Filesystem       Type      Size  Used Avail Use% Mounted on
devtmpfs         devtmpfs  4.0M     0  4.0M   0% /dev
tmpfs            tmpfs     453M     0  453M   0% /dev/shm
tmpfs            tmpfs     182M  464K  181M   1% /run
/dev/nvme0n1p1   xfs       8.0G  1.6G  6.5G  20% /
tmpfs            tmpfs     453M     0  453M   0% /tmp
/dev/nvme0n1p128 vfat       10M  1.3M  8.7M  13% /boot/efi
tmpfs            tmpfs      91M     0   91M   0% /run/user/1000

# '/data' 폴더에 마운트
[ec2-user@ip-10-0-40-220 ~]$ sudo mkdir /data
[ec2-user@ip-10-0-40-220 ~]$ sudo mount /dev/nvme1n1 /data                                                                                                                                                               
[ec2-user@ip-10-0-40-220 ~]$ sudo touch /data/1
[ec2-user@ip-10-0-40-220 ~]$ sudo touch /data/2
[ec2-user@ip-10-0-40-220 ~]$ sudo touch /data/3
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0
-rw-r--r--. 1 root root 0 Jul 25 07:37 1
-rw-r--r--. 1 root root 0 Jul 25 07:38 2
-rw-r--r--. 1 root root 0 Jul 25 07:38 3

# 마운트 해제
[ec2-user@ip-10-0-40-220 ~]$ sudo umount /data

# 마운트를 해제해서 마운트 했을 때 썼던 파일이 사라짐.
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0

# 마운트 후 reboot
[ec2-user@ip-10-0-40-220 ~]$ sudo mount /dev/nvme1n1 /data                                                                                                                                            
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0
-rw-r--r--. 1 root root 0 Jul 25 07:37 1
-rw-r--r--. 1 root root 0 Jul 25 07:38 2
-rw-r--r--. 1 root root 0 Jul 25 07:38 3
[ec2-user@ip-10-0-40-220 ~]$ sudo reboot
[ec2-user@ip-10-0-40-220 ~]$ Connection to 10.0.40.220 closed by remote host.
Connection to 10.0.40.220 closed.

AWSReservedSSO_AdministratorAccess_c3f9c44a58eb4f25:~/environment $ ssh network-01

# reboot 후 파일 정보가 없어짐.
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0

# 볼륨 UUID 확인
[ec2-user@ip-10-0-40-220 ~]$ sudo blkid
/dev/nvme0n1p1: LABEL="/" UUID="765bfc7d-5880-4887-aba3-91f9c0e8091a" BLOCK_SIZE="4096" TYPE="xfs" PARTLABEL="Linux" PARTUUID="843031f9-400c-4f28-bfed-09b691b8312a"
/dev/nvme0n1p128: SEC_TYPE="msdos" UUID="0619-3DE0" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="25463454-fcc0-4bd7-a2b7-07925cc5e420"
/dev/nvme0n1p127: PARTLABEL="BIOS Boot Partition" PARTUUID="1297a52d-d8c4-41b9-b335-41c1fe346356"
/dev/nvme1n1: UUID="74b84cab-9c07-4d00-a84f-2bc0669efb13" BLOCK_SIZE="512" TYPE="xfs"

# /etc/fstab에 nvme1n1의 UUID 저장
[ec2-user@ip-10-0-40-220 ~]$ sudo vim /etc/fstab                                                                                                                                                                         
[ec2-user@ip-10-0-40-220 ~]$ sudo mount -a
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0
-rw-r--r--. 1 root root 0 Jul 25 07:37 1
-rw-r--r--. 1 root root 0 Jul 25 07:38 2
-rw-r--r--. 1 root root 0 Jul 25 07:38 3
[ec2-user@ip-10-0-40-220 ~]$ sudo reboot
[ec2-user@ip-10-0-40-220 ~]$ Connection to 10.0.40.220 closed by remote host.
Connection to 10.0.40.220 closed.
AWSReservedSSO_AdministratorAccess_c3f9c44a58eb4f25:~/environment $ ssh network-01

# reboot 후 network-01에 접속했을 때 마운트가 그대로 되어있음을 알 수 있음.
[ec2-user@ip-10-0-40-220 ~]$ ll /data
total 0
-rw-r--r--. 1 root root 0 Jul 25 07:37 1
-rw-r--r--. 1 root root 0 Jul 25 07:38 2
-rw-r--r--. 1 root root 0 Jul 25 07:38 3
```

### EFS(Elastic File Service)

- 여러 인스턴스에 하나의 볼륨을 연결할 수 있음.
- ip가 생김. security group을 매겨서 사용.
- Storage Class 4가지
    - Standard
    - One-Zone → 1개의 가용영역.
- Performance Mode →
- Throughput Mode →

### EFS 생성 및 연동

![Untitled](Public%20Cloud%2004/Untitled%2035.png)

> 파일 시스템 생성 → 사용자 지정
> 

![Untitled](Public%20Cloud%2004/Untitled%2036.png)

```bash
# EFS를 마운트
sudo mkdir /efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-0638dc862d83bfc88.efs.ap-northeast-2.amazonaws.com:/ /efs
```

> web-server와 network-02에 마운트 후 확인.
> 

![Untitled](Public%20Cloud%2004/Untitled%2037.png)

> /etc/fstab에 저장 후 마운트 해제
> 

```bash
sudo vim /etc/fstab

fs-0638dc862d83bfc88.efs.ap-northeast-2.amazonaws.com:/ /efs    nfs4    nfsvers=4.1,rsize=104857
6,wsize=1048576,hard,timeo=600,retrans=2,noresvport 0 0
```

```bash
sudo umount /efs
# 마운트 해제 후 아무것도 남아있지 않음.
ll /efs 

# /etc/fstab에 있는 내용 전부 마운트
sudo mount -a

# 마운트 후 재확인
ll /efs
total 12
-rw-r--r--. 1 root root 0 Jul 25 08:15 1
-rw-r--r--. 1 root root 0 Jul 25 08:18 test_file.ttt
-rw-r--r--. 1 root root 0 Jul 25 08:15 test_file.txt
```

### S3(Simple Storage Service)

- 인터넷을 이용해 어디서나 접근이 가능하여 보안 중요.
- Bucket & Object
    - 버킷 생성 시 글로벌에서 고유한 이름으로 생성해야 함.
    - 저장되는 데이터마다 FullPath 맵핑
- Storage Class
    - S3 Intelligent-Tiering → 예측하지 못할 때 사용하면 좋음.
    - S3 Standard
    - S3 Standard-IA
    - S3 One Zone-IA → 가용성 확보가 안 됨.
    - S3 Glacier Flexible Retrieval → 저장 비용은 저렴하나 데이터에 접근하는데 매우 오래 걸림.
    - S3 Glacier Deep Archive
    - S3 Glacier Instant Retrieval → 밀리초 단위로 데이터에 접근 가능.
- 로그 데이터와 같이 자주 접근하지 않는 데이터의 경우 S3 Standard나 S3 One Zone-IA, S3 Glacier Instant Retrieval
- Life Cycle Policy
    - 데이터 특성에 맞춰 설정 가능. 자동화 기능 사용 → 객체를 자동으로 다른 Storage Class로 이동시키는 기능.
    - 상위에서 하위 방향은 가능.
- Multipart Upload
- CRR, SRR
    - 백업을 위한 용도로 사용 가능.
    - 해외 리전과 인접한 계정
- Security
    - 버전 관리 기능으로 이전 버전 복구 가능.
    - 삭제 방지 기능.
    - Macie를 연동함으로써 민감한 정보를 자동 탐지함.
