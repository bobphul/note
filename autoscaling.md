
# 1. 목표
> **Autoscale** 가능한 아키텍쳐

# 2. 환경
기본 환경은 AWS를 이용하고, 관련 있는 주요 서비스들은 다음 표와 같다.

| Item | Description |
| ------ | ------ |
| VPC | virtual network environment |
| EC2 | Compute instance(server) |
| AMI | Machine Image |
| Security Group | Network In/Outbound rule |
| ELB | Load Balancer|

## 2.1 샘플 구조
![sample](assets/images/aws-env.jpg)  
[출처] https://bit.ly/2UwnZcD

# 3. 설명

## 3.1 AutoScale

AutoScale의 경우 Scale Out을 통한 서버 대수를 늘려 처리 능력을 향상  
> AWS : EC2 Auto Scaling  
> Azure : Virtual Machine Scale Sets

### 3.1.1 구성요소

1. Groups : 인스턴스 수 지정
2. Configuration templates : AMI, 인스턴스 타입, 키페어, 보안그룹 등
3. Scaling options : 동작 조건 설정

### 3.1.2 동작
![autoscaling Example](assets/images/autoscaling-group.png)  
[출처] https://craig.goddenpayne.co.uk/task-autoscaling-fargate/

## 3.2 AMI
AMI(Amazon Machine Image)는 EC2를 처음 구성할때 지정하는 소프트웨어가 구성된 일종의 템플릿  

AMI 종류
1. AWS 기본 이미지
2. AWS Marketplace 이미지
3. **Custom 이미지**


## 3.3 프로비저닝(Provisioning)

사용자의 요구에 맞게 서버를 설정해 두었다가 필요 시 서버를 즉시 사용할 수 있는 상태로 미리 준비해 두는 것 (feat. wikipedia)  
ex) JDK나 MySQL을 미리 설치하거나 설정

### 3.3.1 도구
#### 3.3.1.1 [Packer](https://packer.io/)
Build Automated Machine Images
1. Builder : 이미지 생성할 플랫폼 지정 (AWS, Azure, Google, docker, vmware, Hyper-V, Naver...)   
2. Provisioner : 이미지 생성할 때 사용할 빌드 도구 (Shell, Ansible, Chef, Salt, Puppet..)
3. Template : Builder + Provisioner 설정 파일 (output:json)

#### 3.3.1.2 [Ansible](https://www.ansible.com/)
**Configuration Management**  
Title Bar : Ansible is Simple IT Automation  
Automation for everyone  

Packer에서도 물론 shell script를 통해서 구성 관리가 가능하나, script 대비 ansible에서는 파일관리, 패키지설치, 시스템관리에 대한 모듈 지원  


#### 3.3.1.3 [terraform](https://www.terraform.io/)
**Infarstructure Provisioning**  
Use Infrastructure as Code to provision and manage any cloud, infrastructure, or service  

계획을 통해 클라우드에 배포(실제적용)전 리소스의 변경되는 내용을 사전 확인 가능

### 3.3.2 Pipeline  
Stamp Pattern  
![Pipeline Example](assets/images/pipeline.png)  
[출처] https://read.acloud.guru/immutable-ami-with-packer-a71694529d60

![Pipeline Example2](assets/images/pipeline2.png)  
[출처] https://blog.gofynd.com/how-we-rebuilt-fynds-infrastructure-3238ec757281


### 3.4 추가사항
Blue/Green 배포 활용  
Scale Out Pattern  
![bluegreen](assets/images/bluegreen.gif)  
[출처] https://datacenterrookie.wordpress.com/2017/05/10/bluegreen-deployment-on-aws/

Weight Transition Pattern  
![route53_weight](assets/images/route53_weight.jpg)  
[출처] https://aws.amazon.com/blogs/startups/upgrades-without-tears-part-2-bluegreen-deployment-step-by-step-on-aws/