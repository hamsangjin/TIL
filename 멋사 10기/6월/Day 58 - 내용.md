# AWS IAM(Identity and Access Management)
`AWS IAM`은 AWS 리소스에 대한 접근을 제어할 수 있는 서비스다. 
IAM을 사용하면 AWS 계정 내의 리소스에 대해 누구에게 어떤 접근 권한을 부여할지를 세밀하게 설정할 수 있다.

## IAM의 주요 구성 요소
- `사용자(User)`: AWS 계정 내에서 리소스에 접근할 수 있는 개별적인 엔터티로, 각 사용자는 고유한 자격 증명(비밀번호, 액세스 키)을 가지며, 이를 통해 AWS 관리 콘솔이나 API에 접근할 수 있다.
    - 사용자는 주로 `개별 직원이`나 `애플리케이션`에 해당한다.
- `그룹(Group)`: 여러 사용자들을 하나의 단위로 묶어 관리할 수 있는 기능으로, 그룹에 정책을 적용하면, 그룹에 속한 모든 사용자에게 동일한 권한이 부여된다.
    - 예를 들어, `개발자` 그룹을 만들어 모든 개발자에게 동일한 권한을 부여할 수 있다.
- `역할(Role)`: 특정 작업을 수행하기 위해 필요한 권한을 정의하는 엔터티로, 역할은 사용자나 서비스에 임시로 부여될 수 있으며, 자격 증명을 필요로 하지 않는다.
    - 역할은 주로 `교차 계정 접근`, `서비스 간 권한 위임`, `EC2 인스턴스에 대한 권한 부여` 등에 사용된다.
- `정책(Policy)`: 권한을 정의하는 JSON 문서로, 정책에는 사용자, 그룹, 역할이 특정 AWS 리소스에 대해 수행할 수 있는 작업을 정의한다.
    - 정책은 크게 `관리형 정책(미리 정의된 정책)`과 `사용자 정의 정책(직접 작성하는 정책)`으로 나눌 수 있다.


## 정책
### JSON을 사용한 정책 작성 방법
- IAM 정책은 JSON 문서 형식으로 작성되며, 다음은 간단한 정책의 예시를 보자.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket", "s3:GetObject"],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

- `Version`: 정책 언어 버전을 명시
- `Statement`: 하나 이상의 문으로 구성되며, 각각의 문은 효과, 작업, 리소스 및 조건을 정의
- `Effect`: 이 문서의 효과를 Allow 또는 Deny로 명시
- `Action`: 허용하거나 거부할 작업을 정의
- `Resource`: 작업이 적용될 리소스를 명시
- `Condition`: 특정 조건 하에서만 문을 적용하는 선택적 요소

<br>

---

<br>

# AWS CLI(AWS Command Line Interface)
`AWS CLI`는 AWS 서비스를 관리하기 위해 명령줄 인터페이스를 제공하는 도구이며, `AWS CLI`를 사용하면 스크립트를 작성하여 AWS 리소스를 자동으로 관리하고, AWS 서비스와 상호 작용할 수 있다.

이를 통해 `AWS Management Console`에서 제공하는 모든 기능을 명령줄에서 수행할 수 있다.

## AWS CLI 명령어 구조
```bash
aws <service> <command> [subcommand] [parameters]
```
- `aws` : AWS CLI를 실행하는 기본 명령어입니다.
- `<service>` : AWS 서비스의 이름을 지정
- `<command>` : 수행할 작업을 지정
- `[subcommand]` : 일부 명령어는 추가 하위 명령어를 가짐
- `[parameters]` : 명령어에 전달할 추가 매개변수

<br>

## 명령어 예제

### AWS CLI 도움말 사용법
```bash
aws help                        # 기본 도움말
aws <service> help              # 특정 서비스 도움말
aws s3 help                     # s3 서비스 도움말
aws <service> <command> help    # 특정 명령어 도움말
aws s3 ls help                  # s3 버킷을 나열하는 명령어에 대한 도움말
```

### 주요 AWS 서비스에 대한 기본 명령어 - s3
오류가 발생한다면 적절한 권한을 추가해줘야한다.
```bash
aws s3 ls                                     # s3 버킷 나열
aws s3 mb s3://my-new-bucket                  # 새 s3 버킷 생성
aws s3 cp myfile.txt s3://my-new-bucket/      # s3 버킷에 파일 업로드
aws s3 cp s3://my-new-bucket/myfile.txt .     # s3 버킷에서 파일 다운로드
aws s3 rb s3://my-new-bucket --force          # s3 버킷 삭제
```

### 주요 AWS 서비스에 대한 기본 명령어 - ec2
```bash
# ec2 인스턴스 나열
aws ec2 describe-instances
# 새 EC2 인스턴스 시작
aws ec2 run-instances
--image-id ami-0b8414ae0d8d8b4cc
--count 1
--instance-type t2.micro
--key-name meet42_keypair
--security-groups launch-wizard-1
--associate-public-ip-address
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=MyInstance}]'
--output json > instance-details.json
# EC2 인스턴스 중지
aws ec2 stop-instances --instance-ids i-0abcdef1234567890
# EC2 인스턴스 시작
aws ec2 start-instances --instance-ids i-0abcdef1234567890
# EC2 인스턴스 종료
aws ec2 terminate-instances --instance-ids i-0abcdef1234567890
```

### 주요 AWS 서비스에 대한 기본 명령어 - IAM
```bash
# IAM 사용자 나열
aws iam list-users
# 새 IAM 사용자 생성
aws iam create-user --user-name carami-user
# IAM 사용자에 정책 첨부
aws iam attach-user-policy --user-name carami-user --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
# IAM 사용자 삭제
aws iam delete-user --user-name newuser
```

#### AWS IAM 사용자 삭제 가이드
AWS IAM 사용자를 삭제할 때, 해당 사용자에게 연결된 모든 정책을 먼저 분리해야 한다. 
사용자가 그룹에 속해 있거나, 인라인 정책이나 관리형 정책이 연결되어 있을 경우 이를 모두 제거해야 한다.
1. 사용자에게 연결된 관리형 정책 분리
2. 사용자에게 연결된 인라인 정책 삭제
3. 사용자 그룹에서 사용자 제거
4. 사용자에 연결된 액세스 키 삭제
5. 사용자에 연결된 MFA 디바이스 삭제
6. 사용자 삭제
