# 인증?? 인가??
![auth](images\security-authentication-user-authorization-websites.png)

# IAM
AWS Identity and Access Management(IAM)는 AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스  
IAM을 사용하여 리소스를 사용하도록 **인증(Authenication)** 및 **권한 부여(Authorization)** 된 대상을 제어  

AWS 계정을 처음 생성할 때는 해당 계정의 모든 AWS 서비스 및 리소스에 대한 완전한 액세스 권한이 있는 단일 로그인 자격 증명 ID로 시작합니다.  
이 자격 증명은 AWS 계정 루트 사용자라고 하며, 계정을 생성할 때 사용한 이메일 주소와 암호로 로그인하여 액세스 하게 됨.  

일상적인 작업은 물론 관리 작업에도 루트 사용자를 사용하지 않는 것이 권장.  
루트 사용자 자격 증명을 안전하게 보관해 두고 몇 가지 계정 및 서비스 관리 작업을 수행할 때만 해당 자격 증명을 사용해야함.  

## 기능
- AWS 계정에 대한 공유 액세스  
암호나 액세스 키를 공유하지 않고도 AWS 계정의 리소스를 관리하고 사용할 수 있는 권한을 다른 사람에게 부여할 수 있습니다.

- 세분화된 권한  
리소스에 따라 여러 사람에게 다양한 권한을 부여할 수 있습니다.

- Amazon EC2에서 실행되는 애플리케이션을 위한 보안 AWS 리소스 액세스  
EC2 인스턴스에서 실행되는 애플리케이션의 경우 IAM 기능을 사용하여 자격 증명을 안전하게 제공 가능.

- 멀티 팩터 인증(MFA)  

- 자격 증명 연동  
기업 네트워크나 인터넷 자격 증명 공급자와 같은 다른 곳에 이미 암호가 있는 사용자에게 AWS 계정에 대한 임시 액세스 권한을 부여할 수 있습니다.

- 자격 증명 정보 보장  
AWS CloudTrail을 사용하는 경우 계정의 리소스를 요청한 사람에 대한 정보가 포함된 로그 레코드를 받게 됩니다.

- PCI DSS 준수  
IAM에서는 전자 상거래 웹사이트 운영자 또는 서비스 공급자에 의한 신용 카드 데이터의 처리, 저장 및 전송을 지원하며, Payment Card Industry(PCI) Data Security Standard(DSS) 준수를 검증 받았습니다.

- 많은 AWS 서비스와의 통합  

- 최종 일관성  
IAM은 다른 많은 AWS 서비스처럼 eventually consistent에 해당됩니다. IAM에서는 전 세계의 Amazon 데이터 센터 내의 여러 서버로 데이터를 복제함으로써 고가용성을 구현합니다. 일부 데이터를 변경하겠다는 요청이 성공하면 변경이 실행되고 그 결과는 안전하게 저장됩니다. 그러나 변경 사항은 IAM에 두루 복제되어야 하고, 여기에는 일정한 시간이 걸립니다. 그러한 변경 사항에는 사용자, 그룹, 역할 또는 정책을 만들거나 업데이트한 것이 포함됩니다. 그러한 IAM 변경 사항을 애플리케이션의 중요한 고가용성 코드 경로에 포함시키지 않는 것이 좋습니다. 대신 자주 실행하지 않는 별도의 초기화 루틴이나 설정 루틴에서 IAM을 변경하십시오. 또한 프로덕션 워크플로우에서 변경 사항을 적용하기 전에 변경 사항이 전파되었는지 확인하십시오.

- 무료 사용  
AWS Identity and Access Management(IAM) 및 AWS Security Token Service(AWS STS)은 추가 비용 없이 AWS 계정에 제공되는 기능입니다. IAM 사용자 또는 AWS STS 임시 보안 자격 증명을 사용하여 다른 AWS 서비스에 액세스하는 경우에만 요금이 부과됩니다.

## IAM 액세스 방법
- AWS Management 콘솔  
콘솔은 IAM 및 AWS 리소스를 관리하기 위한 브라우저 기반 인터페이스입니다.

- AWS 명령줄 도구  
AWS 명령줄 도구를 통해 시스템 명령줄에서 명령을 실행하여 IAM 및 AWS 작업을 수행할 수 있습니다. 명령줄을 사용하는 것이 콘솔을 사용하는 것보다 더 빠르고 편리할 수 있습니다. AWS 작업을 수행하는 스크립트를 작성할 때도 명령줄 도구가 유용합니다.  

        AWS Command Line Interface(AWS CLI)  
        Windows PowerShell용 AWS 도구

- AWS SDK  
AWS에서는 다양한 프로그래밍 언어 및 플랫폼(Java, Python, Ruby, .NET, iOS, Android 등)을 위한 라이브러리와 샘플 코드로 구성된 소프트웨어 개발 키트(SDK)를 제공합니다. 예를 들어 SDK는 요청에 암호화 방식으로 서명, 오류 관리 및 자동으로 요청 재시도와 같은 작업을 처리합니다.

- IAM HTTPS API  
서비스로 직접 HTTPS 요청을 실행할 수 있는 IAM HTTPS API를 사용하여 프로그래밍 방식으로 IAM 및 AWS에 액세스할 수 있습니다.

## 작동방식
![diagram](images\intro-diagram_policies_800.png)

~~### 용어~~  
~~- 리소스  
IAM에 저장된 사용자, 그룹, 정책 및 자격 증명 공급자 객체.   
다른 AWS 서비스와 마찬가지로 IAM에서 리소스를 추가, 편집 및 제거할 수 있습니다.~~  

~~- ID  
식별 및 그룹화에 사용되는 IAM 리소스 객체입니다.  
여기에는 사용자, 그룹 및 역할이 포함됩니다.~~  

~~- 엔터티  
AWS가 인증에 사용하는 IAM 리소스 객체입니다.  
여기에는 IAM 사용자, 연합된 사용자 및 수임된 IAM 역할이 포함됩니다.~~  

~~- 보안 주체  
AWS 계정 루트 사용자, IAM 사용자 또는 IAM 역할을 사용하여 로그인하고 AWS에 요청하는 사람 또는 애플리케이션입니다.~~

~~### 요청~~
~~- 작업 또는 작동 : 보안 주체가 수행하고자 하는 작업 또는 작동입니다.   
AWS CLI 또는 AWS API를 사용하여 AWS Management 콘솔 또는 작동의 작업을 수행할 수 있습니다.~~

~~- 리소스 : 수행된 작업 또는 작동에 따른 AWS 리소스 객체입니다.~~

~~- 보안 주체 : 엔터티(사용자 또는 역할)를 사용하여 요청을 보내는 사람 또는 애플리케이션입니다.  
보안 주체에 대한 정보에는 보안 주체가 로그인하는 데 사용된 엔터티와 관련된 정책이 포함됩니다.~~

~~- 환경 데이터 : IP 주소, 사용자 에이전트, SSL 사용 상태 또는 시간대와 같은 정보입니다.~~

~~- 리소스 데이터 : 요청되는 리소스와 관련된 데이터.  
여기에는 DynamoDB 테이블 이름 또는 Amazon EC2 인스턴스 태그와 같은 정보가 포함될 수 있습니다.~~

### 인증
- 루트 사용자 : 콘솔에서 인증하려면 이메일 주소 및 암호로 로그인  
- IAM 사용자 : 계정 ID 또는 별칭을 입력한 다음 사용자 이름과 암호를 입력  
API 또는 AWS CLI에서 인증하려면 액세스 키 및 보안 키를 제공
- 그룹 : IAM 사용자들의 집합
- 역할 : 사용자와 아주 유사, 역할은 그와 연관된 어떤 자격 증명(암호 또는 액세스 키)이 없다

#### 사용자 vs. 역할
- IAM 사용자가 필요한 경우  
IAM 사용자는 계정에서 특정 권한을 갖는 자격 증명일 뿐이므로 자격 증명이 필요한 모든 경우를 위해 IAM 사용자를 만들 필요는 없다.
AWS 계정을 만들었는데 계정 내에 다른 사람이 없는 경우  
그룹에 속한 다른 사람들이 AWS 계정에서 작업해야 하며 이 그룹이 다른 자격 증명 메커니즘을 사용하고 있지 않는 경우

- IAM 역할이 필요한 경우  
Amazon Elastic Compute Cloud(Amazon EC2) 인스턴스에서 실행되는 애플리케이션을 만들고 그 애플리케이션이 AWS로 요청을 보내는 경우  
휴대폰에서 실행되는 앱을 만들고 그 앱이 AWS로 요청을 보내는 경우  
회사의 사용자들이 기업 네트워크에서 인증을 받았는데 다시 로그인하지 않고도 AWS를 사용할 수 있기를 원합니다. 즉, 사용자들이 AWS로 연동되도록 허용하고 싶습니다.  

### 권한 부여 
1. 기본적으로 모든 요청을 거부 (묵시적 거부, 루트 사용자 증명은 항상 허용)  
2. 권한 정책(자격 증명 기반 또는 리소스 기반)에 포함된 명시적 허용은 이 기본 작동을 재정의
3. 조직 SCP, IAM 권한 경계 또는 세션 정책이 있는 경우 허용이 재정의 함
4. 어떠한 정책의 명시적 거부는 허용을 무시함

![policy-process](images\PolicyEvaluationHorizontal.png)

#### 정책 예제
![sample-policy](images\Types_of_Permissions.diagram.png)  

JohnSmith – John은 Resource X에서 나열 및 읽기 작업을 수행할 수 있습니다. John은 사용자에 대한 자격 증명 기반 정책과 Resource X에 대한 리소스 기반 정책을 통해 이 권한을 부여 받습니다.

CarlosSalazar – Carlos는 Resource Y에서 나열, 읽기 및 쓰기 작업을 수행할 수 있지만 Resource Z에 대한 액세스는 거부됩니다. Carlos의 자격 증명 기반 정책을 통해 Resource Y에서 나열 및 읽기 작업을 수행할 수 있습니다. Resource Y 리소스 기반 정책을 사용하면 Carlos에게 쓰기 권한도 허용됩니다. 그러나 자격 증명 기반 정책을 통해 Resource Z에 대한 액세스가 허용되더라도 Resource Z 리소스 기반 정책으로 인해 해당 액세스가 거부됩니다. 명시적 Deny는 Allow를 재정의하므로 Carlos의 Resource Z에 대한 액세스가 거부됩니다. 자세한 정보는 정책 평가 로직 단원을 참조하십시오.

MaryMajor – Mary는 Resource X, Resource Y 및 Resource Z에 대해 나열, 읽기 및 쓰기 작업을 수행할 수 있습니다. Mary의 자격 증명 기반 정책을 통해 리소스 기반 정책보다 더 많은 리소스에 대해 더 많은 작업을 수행할 수 있지만 액세스를 거부하는 정책은 없습니다.

ZhangWei – Zhang에게는 Resource Z에 대한 모든 액세스 권한이 있습니다. Zhang은 자격 증명 기반 정책이 없지만 Resource Z 리소스 기반 정책을 사용하면 리소스에 대한 전체 액세스 권한을 가질 수 있습니다. Zhang은 Resource Y에서 나열 및 읽기 작업을 수행할 수도 있습니다.

## 사용자 상세설명
### 루트 사용자
AWS 계정의 모든 리소스에 완전히 무제한으로 액세스, 결제 정보에 대한 액세스 및 암호 변경 권한이 포함됨  
이러한 수준의 액세스는 계정을 처음 설정했을 때 필요하며, 일상적인 액세스에서는 사용하지 않는 것이 좋다.  
특히 루트 사용자에 부여된 권한은 제한할 수 없기에 타인과 공유하지 않아야한다.
#### 루트 사용자 자격 증명이 필요한 AWS 작업
- 계정 설정을 변경 (계정 이름, 루트 사용자 암호 및 이메일 주소 포함, 연락처 정보, 결제 통화 기본 설정 및 리전과 같은 기타 계정 설정)
- AWS 지원 계획 변경
- 특정 세금 계산서 조회
- IAM 사용자 권한 복원(IAM 관리자가 실수로 본인의 권한을 취소한다면??)
- 예약 인스턴스 마켓플레이스에 판매자로 등록
- CloudFront 키 페어를 생성 (CloudFront 서명된 URL 또는 서명된 쿠키를 만들때 사용될 키 페어)
- GovCloud 등록
- AWS 계정 close

### IAM 사용자
루트 사용자 자격 증명을 타인과 공유하는 대신, 조직의 사용자에 해당되는 계정 내에 개별 IAM 사용자를 생성할 수 있다.  
IAM 사용자는 별개의 계정이 아니라 해당 계정 내의 사용자임을 알아야 한다.  
즉 AWS 계정 하나에 다수의 사용자를 추가하여 사용할수 있다. 그리고 각 사용자는 고유의 자격 증명을 가지게 된다.  
이떄 일부 사용자는 사실 애플리케이션이 될수도 있다. IAM 사용자가 실제 사람일 필요는 없습니다. AWS 액세스를 필요로 하는 애플리케이션에 대한 액세스 키를 생성하기 위해 IAM 사용자를 생성할 수도 있는 것이다.

### 기존 사용자 연동
1. SAML2.0
2. OpenID Connect(OIDC)

## 정책 및 권한
정책은 자격 증명이나 리소스와 연결될 때 해당 권한을 정의하는 AWS 객체이다.  
정책을 생성하고 IAM 자격 증명(사용자, 사용자 그룹 또는 역할) 또는 AWS 리소스에 연결하여 AWS에서 액세스를 관리한다.  


``` json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "dynamodb:*",
    "Resource": "arn:aws:dynamodb:us-east-2:123456789012:table/Books"
  }
}
```
> Version은 IAM 정책 언어 버전을 이야기함.  
> 2012-10-17 또는 2008-10-17 두가지 버전이 있으며, 오래된 기존 정책은 이 버전이 사용될수 있으며, 새로운 정책이나 기존 정책을 업데이트 하는 경우 이 버전을 사용하면 안된다.

위 예제와 같이 명시적으로 허용되지 않은 작업 또는 리소스는 기본적으로 모두 거부하게 된다. (묵시적거부)  
IAM 콘솔에는 정책에서 각 서비스에 대해 허용되거나 거부되는 액세스 레벨, 리소스, 조건을 설명하는 정책 요약 테이블이 포함되어 있습니다. 
![policy-summary](images\policies-summary-dynamodbexample.png)

정책은 3가지 테이블, 즉 정책 요약, 서비스 요약, 작업 요약으로 요약된다. 
![policy-summary-diagram](images\policy_summaries-diagram.png)

정책 요약은 하나 이상의 Uncategorized services(미분류 서비스), 명시적 거부, 허용 섹션으로 그룹화됨.  
미분류 서비스는 IAM에서 인식하지 못하는 서비스인 경우.  
인식하게되면 명시적 거부 또는 허용에 포함

권한을 부여하는 정책 요약에 나열되어 있는 각 서비스가 서비스 요약  
IAM에서 인식하지 못하는 작업이 정책에 포함되어 있으면 해당 작업은 테이블의 Uncategorized actions(미분류 작업) 섹션에 포함  
IAM에서 작업을 인식하면 해당 작업은 테이블의 액세스 레벨(목록, 읽기, 쓰기, 권한 관리) 섹션 중 하나에 포함  

작업 요약에는 리소스 목록과 선택한 작업에 적용되는 연결 조건이 포함  

정책 Sample  

DenyCustomerBucket
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccess",
            "Effect": "Allow",
            "Action": ["s3:*"],
            "Resource": ["*"]
        },
        {
            "Sid": "DenyCustomerBucket",
            "Action": ["s3:*"],
            "Effect": "Deny",
            "Resource": ["arn:aws:s3:::customer", "arn:aws:s3:::customer/*" ]
        }
    ]
}
```

DynamoDbRowCognitoID
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem"
            ],
            "Resource": [
                "arn:aws:dynamodb:us-west-1:123456789012:table/myDynamoTable"
            ],
            "Condition": {
                "ForAllValues:StringEquals": {
                    "dynamodb:LeadingKeys": [
                        "${cognito-identity.amazonaws.com:sub}"
                    ]
                }
            }
        }
    ]
}
```

Unrecognized_Service_Action
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamobd:*"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Action": [
                "ec2:RunInstances",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:ReboootInstances"
            ],
            "Resource": "*",
            "Effect": "Deny",
            "Condition": {
                "StringEquals": {
                    "ec2:Region": "ap-northeast-2"
                }
            }
        },
        {
            "Action": [
                "ec2:RunInstances",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances"
            ],
            "Resource": "*",
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "ec2:Region": "us-east-2"
                }
            }
        }
    ]
}
```

> Version : 필수, 정책 언어의 버전, 정책 버전과 다름  
> Id : 옵션, 정책 식별자, 일부서비스(SQS, SNS등)에서 UUID(GUID)를 사용하는 경우에 적용  
> Statement : 필수, 정책 주요 요소, 단일문 또는 개별문의 배열을 포함 [{...},{...},{...}]  
> Sid : 옵션, 정책문의 식별자,   
> Effect : 필수, 문의 허용또는 거부  
> Principal : 리소스에 대한 액세스가 허용되거나 거부되는 보안 주체를 지정, 자격 증명 기반 정책에는 사용 안됨.  
> NotPrincipal : 보안 주제 목록에서 예외 지정  
> Action : 특정 작업의 허용 또는 거부, 문에는 `Action`또는 `NotAction`은 반드시 추가  
> NotAction : 지정된 작업의 목록을 제외한 모든 작업을 명시적으로 적용된 정책과 일치  
```json
"Effect": "Allow",
"NotAction": "s3:DeleteBucket",
"Resource": "arn:aws:s3:::*",
```
```json
"Effect": "Allow",
"NotAction": "iam:*",
"Resource": "*"
```
```json
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "DenyAllUsersNotUsingMFA",
        "Effect": "Deny",
        "NotAction": "iam:*",
        "Resource": "*",
        "Condition": {"BoolIfExists": {"aws:MultiFactorAuthPresent": "false"}}
    }]
}
```
> Resource : 문에서 다루는 객체를 지정, 문에는 `Resource` 또는 `NotResource`는 반드시 추가  
> NotResource : 지정된 리소스를 제외한 모든 리소스와 명시적으로 적용된 정책과 일치  
```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Deny",
    "Action": "s3:*",
    "NotResource": [
      "arn:aws:s3:::HRBucket/Payroll",
      "arn:aws:s3:::HRBucket/Payroll/*"
    ]
  }
}
```
> Condition : 옵션, 정책의 효력이 발생하는 시점에 대한 조건 지정  
```json
"Condition" : { "{condition-operator}" : { "{condition-key}" : "{condition-value}" }}
```

Contidion 조건 연산자(문자열, 숫자, 날짜, 부울, 이진, IP, ARN, IfExists) :  
https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html

### 자격 증명 기반 정책 및 리소스 기반 정책
자격 증명 기반 정책 : IAM 사용자, 그룹 또는 역할과 같은 IAM 자격 증명에 연결할 수 있는 권한 정책  
리소스 기반 정책 : Amazon S3 버킷 또는 IAM 역할 신뢰 정책과 같은 리소스에 연결하는 권한 정책

자격 증명 기반 정책은 자격 증명이 수행할 수 있는 작업, 대상 리소스 및 이에 관한 조건을 제어한다.  

    관리형 정책 : 
    AWS 계정에 속한 다수의 사용자, 그룹 및 역할에게 독립적으로 연결할 수 있는 자격 증명 기반 정책입니다. 사용할 수 있는 관리형 정책은 두 가지가 있음
    
        AWS 관리형 정책 : 
        AWS에서 생성 및 관리하는 관리형 정책. 
        정책 사용이 처음이라면 AWS 관리형 정책 사용을 먼저 권장
![aws-managed](images\policies-aws-managed-policies.diagram.png)

        고객 관리형 정책 : 
        사용자가 자신의 AWS 계정에서 생성 및 관리하는 관리형 정책  
        고객 관리형 정책은 AWS 관리형 정책보다 정책에 대해 더욱 정밀하게 제어   
        시각적 편집기에서 또는 JSON 정책 문서를 직접 생성하여 IAM 정책을 생성 및 편집 가능  

![custom-policy](images\policies-customer-managed-policies.diagram.png)

    인라인 정책 :  
    자신이 생성 및 관리하며, 단일 사용자, 그룹 또는 역할에 직접 포함되는 정책  
    대부분의 경우 인라인 정책을 사용하지 않는 것 권장

![inline-policy](images\policies-inline-policies.diagram.png)

    인라인보다 관리형 권장 이유
    1. 재사용성
    2. 중앙 변경 관리
    3. 버전 관리 및 롤백
    4. 권한 위임 관리
    5. AWS 관리

리소스 기반 정책은 지정된 보안 주체가 해당 리소스에 대해 수행할 수 있는 작업 및 이에 관한 조건을 제어한다.  
리소스 기반 정책은 인라인 정책이며, 관리형 리소스 기반 정책은 없습니다.  
교차 계정 액세스를 활성화하려는 경우 전체 계정이나 다른 계정의 IAM 엔터티를 리소스 기반 정책의 보안 주체로 지정할 수도 있습니다.  

[reference] https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access_policies_identity-vs-resource.html

### 정책 및 계정
AWS에서 하나의 계정을 관리하려면 정책을 사용하여 해당 계정 내 권한을 정의함.  
여러 계정 전반의 권한을 관리하고자 한다면 사용자에 대한 권한을 관리하기가 더 어렵습니다.  
교차 계정 권한에 대해 IAM 역할, 리소스 기반 정책 또는 액세스 제어 목록(ACL)을 사용할 수 있습니다.  
이때 사용하는게 AWS Organizations 서비스이다.

### 정책 및 사용자
IAM 사용자는 서비스의 자격 증명이다.  
IAM 사용자를 생성할 경우, 권한을 부여하지 않는 한 사용자는 계정 내에서 어떠한 것으로도 액세스할 수 없게되며,  
사용자 또는 사용자가 속한 그룹에 연결된 정책인 자격 증명 기반 정책을 생성하여 사용자에게 권한을 부여하게 된다.

### 정책 및 그룹
IAM 사용자를 IAM 그룹으로 구성하고 그룹에 정책을 연결할 수 있다.  
각 사용자는 별도의 자격 증명을 갖고 있을수도 있지만, 그룹에 연결된 정책에 명시된 권한이 그룹 내 모든 사용자에게 부여되게 된다.   

![iam-diagram](images\iam-intro-users-and-groups.diagram.png)


### 정책 예제

[referance] https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access_policies_examples.html

## ABAC(Attribute-based access control)
속성을 기반으로 권한을 정의하는 권한 부여 전략이며, AWS에서는 이러한 속성을 태그라고 함  
ABAC 정책은 보안 주체의 태그가 리소스 태그와 일치할 때 작업을 허용하도록 설계될 수 있다.  

![abac](images\tutorial-abac-concept.png)

[reference] https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html

RBAC(role-based access control)은 IAM에서 사용되던 기존 권한 부여 모델이다.  
RBAC는 AWS 외부에서 역할로 알려진 개인의 직무에 따라 권한을 정의한다.
IAM에서는 다양한 직무에 대해 서로 다른 정책을 생성하여 RBAC를 구현하며, 정책을 자격 증명(IAM 사용자, 사용자 그룹 또는 IAM 역할)에 연결하게 됨.  
가장 좋은 방법은 직무에 필요한 최소 권한을 부여하는 것.  
직원이 새 리소스를 추가할 때 해당 리소스에 액세스할 수 있도록 정책을 업데이트해야 한다.

![rbac](images\tutorial-abac-rbac-concept.png)

RBAC 모델에 비해 ABAC의 이점

- ABAC 권한은 혁신적으로 확장됩니다.  
관리자가 새 리소스에 액세스할 수 있도록 기존 정책을 업데이트할 필요가 없습니다.  
예를 들어, access-project 태그로 ABAC 전략을 설계했다고 가정합니다. 개발자는 access-project = Heart 태그와 함께 역할을 사용합니다. Heart 프로젝트의 사람에게 추가 Amazon EC2 리소스가 필요한 경우 개발자는 access-project = Heart 태그를 사용하여 새 Amazon EC2 인스턴스를 생성할 수 있습니다. 그러면 Heart 프로젝트의 모든 사용자가 태그 값이 일치하기 때문에 해당 인스턴스를 시작하고 중지할 수 있습니다.

- ABAC를 사용하면 필요한 정책 수가 적어집니다.  
각 직무에 대해 서로 다른 정책을 생성할 필요가 없기 때문에 생성해야 하는 정책이 더 적습니다. 그러므로 정책을 관리하기가 더 쉽습니다.

- ABAC를 사용하여 팀은 빠르게 변화하고 성장할 수 있습니다.  
새 리소스에 대한 권한이 속성에 따라 자동으로 부여되기 때문입니다.  
예를 들어, 회사에서 이미 ABAC를 사용하여 Heart 및 Sun 프로젝트를 지원하는 경우 새 Lightning 프로젝트를 쉽게 추가할 수 있습니다. IAM 관리자는 access-project = Lightning 태그를 사용하여 새 역할을 생성합니다. 새 프로젝트를 지원하기 위해 정책을 변경할 필요는 없습니다. 역할을 맡을 권한이 있는 모든 사용자는 access-project = Lightning 태그가 지정된 인스턴스를 생성하고 볼 수 있습니다. 또한 팀 멤버가 Heart 프로젝트에서 Lightning 프로젝트로 이동할 수 있습니다. IAM 관리자는 사용자를 다른 IAM 역할에 할당합니다. 권한 정책을 변경할 필요가 없습니다.

- ABAC를 사용하여 세분화된 권한을 사용할 수 있습니다.  
정책을 생성할 때는 최소 권한을 부여하는 것이 가장 좋습니다. 기존 RBAC를 사용하는 경우에는 특정 리소스에 대한 액세스만 허용하는 정책을 작성해야 합니다. 그러나 ABAC를 사용하는 경우 리소스 태그가 보안 주체의 태그와 일치하는 경우에만 모든 리소스에 대한 작업을 허용할 수 있습니다.

- ABAC를 사용하여 회사 디렉터리의 직원 속성을 사용합니다.  
세션 태그를 AWS에 전달하도록 SAML 기반 또는 웹 자격 증명 공급자를 구성할 수 있습니다. 직원이 AWS에 연동되면 해당 속성이 AWS의 결과 보안 주체에 적용됩니다. 그런 다음 ABAC를 사용하여 이러한 속성에 따라 권한을 허용하거나 거부할 수 있습니다.

**[AWS re:Inforce 2019: Scale Permissions Management in AWS w/ Attribute-Based Access Control (SDD350)]**  
https://www.youtube.com/watch?v=Iq_hDc385t4

# 설정
## 액세스 유형
1. AWS 계정 내 사용자에 대한 액세스
2. AWS 계정 간 교차 계정 액세스  
https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html
3. 권한 부여 시스템과 AWS 사이의 자격 증명 연동을 통한 AWS 사용자가 아닌 사용자의 액세스  

## 로그인 페이지 설명

https://console.aws.amazon.com/  
https://My-AWS-Account-ID.signin.aws.amazon.com/console/  

## 실습

1. AWS 계정 별칭 설정
2. 루트 사용자 MFA 활성화
3. 그룹 추가
4. 사용자 추가
5. 패스워드 정책과 권한 확인
6. 검색 (User, Group, role, admin)
7. aws-cli  
aws-cli : https://aws.amazon.com/ko/cli/  

### 참고URL
AWS Policy Generator
http://awspolicygen.s3.amazonaws.com/policygen.html

IAM 정책 시뮬레이터  
https://policysim.aws.amazon.com/home/index.jsp?#users

IAM 보안 모범 사례  
https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/best-practices.html