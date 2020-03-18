---

layout: post
title: "AWS 클라우드 보안을 위한 IAM"
description: "IAM의 Policy와 Role, 그리고 IAM 모범사례 TOP 10"
date: 2020-03-18
tags: [AWS, IAM, Role, Policy, 클라우드보안]
comments: true
share: true

---

**목차**

---

* toc
{:toc}

---

## 1. AWS 서비스를 사용하는 방법 

1. Console 기반
   - Console 기반으로 작업할 경우 쉽게 사용할 수 있지만, 반복작업이 어렵고 시간이 오래걸림

2. Script 기반
   - Script 기반으로 작업할 경우 반복작업에 적합하고, Script 작성자의 역량에 따라 원하는 항목에 대한 수정이 용이하다. 하지만 리소스의 준비상태 확인이나 문제 발생시 복원이 어렵다는 단점을 가지고 있음.

3. 프로비저닝 엔진(AWS CloudFormation, Terraform)
   - 자동화 구현에 용이
   - 반복작업에 적합
   - 에러 발생 시 원복이 쉬움
   - 최초 구현이 어려움
   - AWS ColudFormation template(JSON/YAML) → AWS CloudFormation
   - 3rd Party Engine(HashiCorp Configuration Language(HCL)) → Terraform

4. DOM 모델 적용
   - 코드 기반으로 사용 가능
   - 원하는 형상에 대한 정의
   - 초기 코딩의 복잡성

5. CDK(Cloud Development Kit) 사용

<br>

**결국 중요한 건 API!!**, API를 보호하기 위해 AWS는 SigV4를 이용하고 있다. 시간, 리전, 어떤 서비스를 호출하는지 등의 정보를 API 호출에 함께 전송한다. CLI나 SDK 사용 시 자동으로 포함되어 전송되기 때문에 따로 신경쓸 필요가 없지만, 별도로 개발하여 이용할 경우 데이터 전송 시 Signature를 생성해 같이 전송해야 한다.

<br>

## 2. AWS의 AAA(Authentication, Authorization, Audit)

**인증(Authentication) → 신원확인**

Authentication ↔︎ AWS IAM
사용자 이름/PW(+MFA;Multi Factor Authentication), Access Key(+MFA), Federation 등을 통해 인증을 수행

<br>

**인가(Authorization) → 권한확인**

AWS IAM ↔︎ Authorization
Policy를 통해 인가를 확인

<br>

**감사(Audit) → 기록**

CloudTrail 서비스를 이용함

<br>

## 3. IAM Policy

AWS 서비스와 리소스에 대한 인가 기능을 제공한다.

Policy를 정의할 때 어떤 IAM Principal이 어떤 Condition에서 AWS의 어떤 Resource에 대해 어떤 Action을 허용 혹은 차단할 것인지를 지정

IAM이 정의한 Policy를 기반으로 API 요청을 검사/평가하게되며 최종적으로 허용 혹은 차단을 결정하게된다.

<br>

**IAM으로 할 수 있는 것은?**

1. AWS 서비스와 리소스에 대한 액세스를 안전하게 관리가 가능함
2. AWS 사용자 및 그룹을 만들고 관리할 수 있음
3. 권한을 사용해 AWS 리소스에 대한 액세스를 허용 및 거부가 가능함

<br>

**IAM의 장점**

1. AWS 리소스에 대한 사용자의 액세스를 세부적으로 제어 가능
2. 대부분의 AWS 서비스 내에 통합되어 있음
3. 유연한 보안 자격 증명 관리
4. 기존 자격 증명 시스템을 활용할 수 있음(Federation)
5. AWS 계정에서 추가 비용 없이 사용 가능

<br>

### IAM Policy의 종류와 사용 목적

S3 ACL(S3 ACL은 XML 형식 사용)을 제외한 모든 Policy는 JSON 형식의 동일 문법을 사용한다.

1. AWS Organizations
   - Service Control Policies(SCPs)
   - 어카운트 내의 특정 Principal에 대한 서비스 제어

2. AWS Identity and Access Management(IAM)
   - Permission Policies, Permission Boundaries
   - IAM Principal(Users, Roles)에 대한 상세 권한 설정 및 사용 가능한 권한 경계

3. AWS Security Token Services(AWS STS)
   - Session Policies
   - 역할 전환이나 Federation 시 권한을 제어

4. Specific AWS Services
   - Resource-based Policies
   - 다수 계정 접속이나 서비스로부터의 접근을 제어

5. VPC Endpoints
   - Endpoint Policies
   - VPC Endpoint에서 서비스로의 접근을 제어

<br>

IAM Policy에는 두 가지 기반의 정책이 있다.

1. Identity 기반 정책
   - SCP, Permission Policy, Permission Boundary(Permission Policy의 범위를 제한하는 설정), Session Policy
   - Policy 만든 후 사용자에게 할당

2. Resource 기반 정책
   - Bucket Policy, Access Control List
   - Policy 만든 후 리소스에 할당

<br>

### IAM Policy의 구조

```
{
  "Statement":[{
    "Effect":"Allow 또는 Deny",
    "Principal":"principal"
    "Action":"action",
    "Resource":"arn",
    "Condition":{
      "condition":{
        "key":"value"
        }
      }
    }
  ]
}
```

|항목|설명|
| :----: | ---- |
| Effect | 명시된 정책에 대한 허용 혹은 차단|
| Principal | 접근을 허용 혹은 차단하고자 하는 대상 Account, User, Role 등|
| Action | 허용 혹은 차단하고자하는 접근 타입 <br> "Action":"s3:GetObject"|
| Resource | 요청의 목적지가 되는 서비스 <br> "Resource":"arn:aws:sqs:us-west-2:123456789012:queue1" |
| Condition | 명시된 조건 유효하다고 판단될 수 있는 조건, IP, 날짜, API Call의 에이전트 정보 등 <br> "StringEqualsIfExists":{"aws:RequestTag/project":["Pickles"]}|

<br>

**Policy를 쉽게 활용하기 위한 도구**

1. Policy Generator : AWS IAM 콘솔에서 GUI를 바탕으로 간단하게 AWS IAM 정책을 만들 수 있음
2. Policy Simulator : 계정 내의 다양한 리소스들에 대하여 만들어둔 정책들을 테스트해 볼 수 있음

<br>

## 4. IAM 권한 할당 원리의 이해

1. 명시적 Deny, 묵시적 Deny
- API 요청 시 조건이 Deny로 설정되어있으면 명시적 Deny
- 별도로 허용하지 않아도 Default가 Deny로 되어있는 것을 묵시적 Deny(Like, ANY ANY DENY)

2. 명시적 허용
- 명시적 Deny가 없어야하고 Permission Boundary와 Permission Policy에 명시적으로 허용되어 있어야 함.  

3. 권한을 확인하는 순서
- 명시적 Deny → SCP → Permission Boundary → Permission Policy → Resource Policy

4. 권한의 획득 조건
- Or 조건 : 중첩된 조건을 모두 허용하게 됨.
    - Permission Policy, Resource Policy
- And 조건 : 조건이 겹쳐지는 부분만을 허용하게 됨.
    - Permission Boundary, Permission Policy
    - Permission Boundary, Permission Policy, Service Control Policy, Session Policy

<br>

## 5. IAM Role이란?

- AWS의 작업과 리소스에 대한 액세스를 부여하는 권한 세트
- 정의된 권한을 다른 사용자나 서비스로 위임
- AWS IAM users 또는 AWS services는 role에서 정의된 권한범위 내 AWS API를 사용할 수 있는 'temporary security credentials'를 얻을 수 있음

<br>

**Role을 사용할 때의 장점**
1. 보안자격증명을 공유할 필요가 없기 때문에 보안이 향상된다.(임시보안자격증명 사용)
2. 언제든지 접근 권한 회수 가능하여 강력하게 권한을 제어할 수 있다.
3. 각 사용자에게 매번 필요한 권한을 일일이 부여할 필요가 없다.
4. 별도의 과금이 없다.
- SAML 2.0 기반 연동을 사용하면 Federation SSO이 가능해서 AWS IAM사용자를 일일이 만들지 않고도 AWS Console에 로그인하거나 API 호출이 가능함.

<br>


<br>

**IAM 정책 생성 자동화**

1. Access Advisor를 활용한 미사용 권한 탐지

- Access Advisor
    - 최대 1년간의 기간동안 저장된 데이터 기준으로 마지막 접속 서비스에 대한 정보를 제공
    - Access Advisor 정보를 API를 통해 조회 가능(자동화가 가능해짐)
- 오픈 소스를 활용한 IAM 권한 관리
    - Ardvark : IAM의 Access Advisor 정보를 조회
    - Repokid : 불필요한 권한 삭제
   
<br>

## 6. Audit

- CloudTrail을 이용해 모든 Log를 모아 한곳으로
   - 유저를 6가지로 나누어 기록
      - Root/AWS IAM User/Assume Role/Federated User/Other Account/AWS Services
- AWS IAM과 통합
- AWS 계정 단위로 이벤트 기록
- AWS에 요청되어 승인된 모든 API Call들을 로깅
- 모든 리전의 로그를 하나의 S3 Bucket에 통합 저장
- AWS KMS를 통해 로그 파일 암호화 가능
- 위변조에 대비하기 위해 Log File Integrity(LFI)체크 가능


<br>

## 7. AWS IAM 모범사례 TOP10

### 1. Users - 개별 사용자 생성
**Do**
- 관리자 스스로도 AWS IAM 사용자 생성하여 그것을 이용
- 다른 사람들에게도 각각 사용자 생성하여 이용하도록 함

**Benefit**
- 개별 자격증명 세트 관리
- 개별 권한 부여
- 세밀한 제어 가능
- 접근 권한 회수가 쉬움

<br>

### 2. Password - 강력한 암호정책 설정
**Do**
- 암호 만료 기한 설정
- 암호 정책은 강력하게

**Benefit**
- 사용자와 그 데이터가 안전하게 보호됨
- 암호 복잡도에 대한 요구사항을 쉽게 만족시키고 적용 가능
- Brute Force 로그인 시도에 대한 안전도 상승

<br>

### 3. Rotate - 보안 자격 증명을 정기적으로 순환
**Do**
- AWS IAM users에 대해 보안 자격 증명 활성화
- 보안 자격 증명 순환 여부 확인/감사를 위해 Credential Report 를 활용
- Credential Report의 Access Key Last Used 컬럼을 통해 일정 기간동안 사용되지 않은 자격증명을 찾아내고 비활성화 시킴

**Benefit**
- 인가되지 않은 접근의 가능성을 최대한 낮춰줌
- 오래된 키가 도난당하거나 잃어버린 상태라고 해도 그 키를 통해 데이터에 접근하는 것을 방지할 수 있음

<br>

### 4. MFA - 권한이 있는 사용자에 대해 MFA 활성화
**Do**
- Root account에 대하여 MFA활성화
- 민감한 action들에 대해서는 MFA로 보호

**Benefit**
- 추가적인 보호막 제공
- 콘솔과 programmatic 접근에 대한 보안 향상

<br>

### 5. Groups - 권한 관리에 그룹 사용
**Do**
- 업무 기능과 연관된 그룹을 생성
- 그룹에 정책들을 붙임
- 그룹 멤버십을 권한 부여/회수하는데 사용

**Benefit**
- 사용자 증가에 따라 늘어나는 접근 제어 관리의 복잡도를 감소시킬 수 있음
- 사용자들이 갑자기 의도치 않게 과한 접근권한을 얻게 되는 일을 방지하거나 줄임
- 현재 직군의 role이 변경되었을 경우 쉽게 권한을 다시 할당
- 여러 사용자의 권한을 업데이트할 수 있는 쉬운 방법

<br>

### 6. Permissions - 최소한의 권한만 부여
**Do**
- 최소한의 권한 세트만으로 시작한 후에 필요에 따라 권한을 추가
- 특권에 대해서는 Conditions을 이용해 접근 제어
- 정기적으로 Access Advisor를 체크하여 권한을 제한
- Resource-based 정책을 이용해 특정 resource에 대한 접근 제어

**Benefit**
- 실수로 특권을 행사할 가능성을 최소화
- 서서히 풀어주는 것이 갑자기 조이는 것보다 쉬움
- 좀 더 정교한 제어 가능

<br>

### 7. Sharing - 접근제어를 공유하기 위해 AWS IAM Role 사용
**Do**
- 다음의 경우 Role 사용
  - 계정 간 접근 권한 위임
  - 동일 계정 내에 접근권한 위임
  - 연동된 사용자

**Benefit**
- 보안 자격증명을 더 이상 공유할 필요 없음
- 장기 자격증명을 보관할 필요 없음
- 누가 접근권한이 있는지를 제어함

<br>

### 8. Roles - Amazon EC2 instances에 AWS IAM Role 사용
**Do**
- 장기 자격증명을 사용하지 말고 Role을 사용
- 어플리케이션에 최소한의 권한만을 부여

**Benefit**
- EC2 instances 상의 access keys를 관리하기 쉬움
- Key 순환의 자동화
- AWS SDKs와 완전히 통합
- AWS CLI와 완전히 통합

<br>

### 9. Auditing - API호출 로그를 얻기 위해 AWS CloudTrail 활성화
**Do**
- 모든 리전에서 AWS CloudTrail 를 활성화
- AWS CloudTrail에서 Log File Validation가 활성화 되어 있는 것을 확인
- CloudTrail 로그를 저장하는 Amazon S3 버킷이 퍼블릭하게 접속 가능하지 않도록 설정

**Benefit**
- 각 계정의 API활동내역을 모니터링 할 수 있음
- 보안 분석, 리소스 분석, 컴플라이언스 감사 등을 가능하게 함

<br>

### 10. Root - Root 계정의 사용을 줄이거나 없앰
**Do**
- Root account 사용자에 대한 MFA 활성화
- 가능하면, root access keys를 삭제
- 각 계정들에 대하여 안전한(강력한) 비밀번호 사용

**Benefit**
- 우발적인 변경 및 고도의 권한이 부여 된 자격증명의 의도하지 않은 노출과 같은 위험을 줄임

<br>

## 참고자료
- [IAM 정책을 잘 알아야 AWS 보안도 쉬워진다. 이것은 꼭 알고 가자! - 신은수 솔루션즈 아키텍트(AWS)](https://www.youtube.com/watch?v=iPKaylieTV8)
- [클라우드 여정을 성공적으로 수행하기 위한 AWS IAM 활용 전략 - 최원근 솔루션즈 아키텍트(AWS)](https://www.youtube.com/watch?v=e1lV4lyNWGg)
