# Public Cloud 05

## Database System

### RDS

### DynamoDB

### ElasticCache

## Authentication Service

### IAM

- IAM User → 보안을 준수하기 위해 권고
- IAM User에 있는 access key 사용을 최소화하는 것이 좋음.
- STS Token → 일정 시간이 지나면서 갱신해야 하므로 보안적으로 좋음.
- IAM User로 콘솔에 접근하면 세션을 끊을 수 없음.
- IAM Role로 콘솔에 접근하면 시간을 정해서 세션을 끊을 수 있도록 함.
- Policy
    - principal에서 Role의 신뢰 정책 정의.
    - 자격 증명 기반 정책
        - AWS 관리형 정책
        - 고객 관리형 정책
        - 인라인 정책 →
    - 리소스 기반 정책 →

### Password Policy & MFA

- MFA → 2차 인증 방식을 선택해서 할 수 있음.

### Organizations

- 통합해서 관리할 수 있는

### Another Security Service

- Identity Center
- Directory Service
- Cognito
- Control Tower

## Logging System

### CloudTrail

- 저장소를 지정해 저장 가능.

### ELB Access Log

### VPC Flow Log

- ELB의 web으로 가는 거나 db로 가는 로그도 저장 가능.
- 로깅 되는 것이 많다면 S3에 저장하는 것이 좋음.

## Security Service

### AWS Backup

- 중앙에서 백업 가능한 모든 리소스를 대상으로 관리.
- 계정이 여러 개여도 Organazation으로 관리하면 중앙에서 전체 리소스를 백업하는 것을 지원.

### AWS WAF(Web Application Firewall)

- 인프라 구성에 변경 없이 추가 가능.
- 순서를 잘 지켜서 규칙을 적용해야 함.
- 모든 트래픽을 수집 → 트래픽 자체에 패턴 파악 → Block 규칙을 추가함.
- 초기에는 count 상태로 많이 설정함.
- 차단 규칙을 추가할 때는 Staging이나 QA에서 진행.
- WAF 생성 후 WEB ACL 구성.
- Capacity 최대치 5000. 그 이상은 적용 불가.

### AWS Shield

### AWS Secrets Manager

### ACM(AWS Certificate Manager)

- public → 콘솔 내. 기한이 있어서 update 기간이 되면 자동으로 갱신해줌. 아마존 내에서만 사용 가능.
- private → 콘솔 내에서 만드는 것은 같으나 내부에서 사용할 때.
- Import → 외부에서 구매해서 등록. update 기간이 되면 자동으로 갱신이 되지 않아 사용자가 갱신하거나 새로 발급 받아서 등록해야 함.

## CloudWatch

### Metric

- CPU Utilization과 같이 하나의 지표.
- Period → 집계 간격.

### Log Group

## CloudFormation

### Terraform & CloudFormation

- Terraform → 모듈 단위로 나눠서 작업할 때 가독성이 좋음.
- CloudFormation →

## Amazon S3 생성 및 웹 호스팅 설정

![Untitled](Public%20Cloud%2005/Untitled.png)

![Untitled](Public%20Cloud%2005/Untitled%201.png)

![Untitled](Public%20Cloud%2005/Untitled%202.png)

새 OAI 생성 → CloudFront가 ID를 생성해줌.

Athena → S3에 있는 데이터 검색할 때 사용.
