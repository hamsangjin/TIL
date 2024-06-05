# 아마존 웹 서비스(Amazon Web Service) 개요
아마존 웹 서비스(AWS)는 전 세계적으로 가장 광범위하게 사용되는 클라우드 컴퓨팅 플랫폼 중 하나이다.

## 컴퓨팅 서비스
- `Amazon EC2(Elastic Compute Cloud)`: 가상 서버를 제공하여 사용자가 컴퓨팅 용량을 유연하게 확장하거나 축소할 수 있다.
- `AWS Lambda`: 서버리스 컴퓨팅 서비스로, 코드를 작성하고 AWS가 실행, 확장성 관리, 서버 유지보수를 자동으로 처리한다.

## 네트워킹 서비스
- `Amazon VPC (Virtual Private Cloud)`: 사용자가 정의한 가상 네트워크에서 AWS 리소스를 시작할 수 있게 해주며, 보안 및 네트워크 구성을 완벽하게 제어할 수 있다.
- `Amazon Route 53`: 확장 가능한 도메인 이름 시스템 (DNS) 웹 서비스로, 도메인 이름을 IP 주소로 변환합니다.

## 스토리지 서비스
- `Amazon S3(Simple Storage Service)`: 데이터를 저장하고 검색할 수 있는 객체 스토리지 서비스로, 웹 사이트 콘텐츠, 백업, 아카이브 등에 사용된다.
- `Amazon EBS(Elastic Block Store)`: EC2 인스턴스에 사용할 수 있는 고성능 블록 스토리지를 제공한다.

## 데이터베이스 서비스
- `Amazon RDS (Relational Database Service)`: 관리형 관계형 데이터베이스 서비스로, MySQL, PostgreSQL, Oracle, SQL Server 및 Amazon Aurora를 포함한 주요 데이 터베이스 엔진을 쉽게 설치, 운영 및 확장할 수 있다.
- `Amazon DynamoDB`: 완전 관리형 NoSQL 데이터베이스 서비스로, 모바일, 웹, 게임, IoT 등의 애플리케이션을 위한 빠르고 일관된 성능을 제공한다.

## 분석 플랫폼
- `Amazon Redshift`: 완전 관리형, 페타바이트 규모의 데이터 웨어하우스 서비스로, 기존 데이터 웨어하우스보다 더 저렴한 비용으로 빅데이터 세트를 분석할 수 있다.
- `AWS Glue`: 서버리스 데이터 통합 서비스로, 데이터 준비 및 로딩을 간소화하여 분석 및 머신러닝을 쉽게 할 수 있게 해준다.

## 애플리케이션 서비스
- `Amazon SNS(Simple Notification Service)`: 고도로 확장 가능한, 완전 관리형 푸시 알림 서비스로, 애플리케이션에서 사용자에게 푸시 알림을 보낼 수 있다.
- `AWS Elastic Beanstalk`: 웹 애플리케이션과 서비스를 쉽게 배포하고 확장할 수 있는 서비스로, 인프라 관리의 복잡성 없이 애플리케이션에 집중할 수 있다.

<br>

# EC2와 EBS를 이용해서 나만의 Linux 서버 만들기

## 보안 그룹 생성 - 인바운드 규칙만 추가
![image](https://github.com/hamsangjin/TIL/assets/103736614/73b489de-968e-4c92-a0b8-ff2a3228bac3)

## 인스턴스 생성
![](https://github.com/hamsangjin/TIL/assets/103736614/cdd345b3-23ce-48b6-ae3c-7ec1dbee31d3)
![](https://github.com/hamsangjin/TIL/assets/103736614/d35bef7d-669b-4b10-b3fd-5fb4830dfe24) | ![](https://github.com/hamsangjin/TIL/assets/103736614/1190bb05-1703-4d4c-a800-60bdcefdac4c)
-- | -- |

![](https://github.com/hamsangjin/TIL/assets/103736614/d3ca2d03-43ec-4679-9f67-a9ef2aaf7bd3)
![](https://github.com/hamsangjin/TIL/assets/103736614/72897ef7-4b57-47db-8111-964194ac2fa3)

<br>

---

<br>

# SSH 접속
```bash
chmod 400 sangjin_keypair.pem
ssh -i "sangjin_keypair.pem" ec2-user@퍼블릭IP
```

![](https://github.com/hamsangjin/TIL/assets/103736614/e52e0a91-a64e-4143-8fe0-6290d131d548)

<br>

---

<br>

# Git, Java 설치

## Git 설치
![](https://github.com/hamsangjin/TIL/assets/103736614/6ffce434-0ae5-46af-8f8a-c4709fc46d26)

## Git 설정
![](https://github.com/hamsangjin/TIL/assets/103736614/31d1abc7-8c3c-4ba0-ac39-2a9b912e159d)

## Amazon Corretto 21 설치
![](https://github.com/hamsangjin/TIL/assets/103736614/ec9e1aa3-83bc-43ea-9d2a-ff88649ddf61)

## JDK 설치 확인
![](https://github.com/hamsangjin/TIL/assets/103736614/99286f9b-ed98-47a8-b5ee-e79f2dcc27f3)

<br>

---

<br>

# Spring Boot 프로젝트 내려받고 빌드하기

## git clone
![](https://github.com/hamsangjin/TIL/assets/103736614/c0582069-579a-4fff-ad26-af40d4212565)

## gradlew 빌드 실행
![](https://github.com/hamsangjin/TIL/assets/103736614/dfa98da1-f254-4ba0-b44f-c852af7a95a4)

<br>

---

<br>

# 서버 실행하기
## jar파일 실행
![](https://github.com/hamsangjin/TIL/assets/103736614/990cea44-26fd-401e-b4b8-faa290eb6ad6)

## 내 컴퓨터 8080 port에 대한 인바운드 규칙 설정
![](https://github.com/hamsangjin/TIL/assets/103736614/877ce626-1a3b-43e7-a645-35dd7b4d9858)

## 접속 화면
![](https://github.com/hamsangjin/TIL/assets/103736614/bbeeb8e6-6a6c-4353-8ae6-0da812311040)
