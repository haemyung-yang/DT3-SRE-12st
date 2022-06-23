# Cloud Transformation / 10790 양해명
## user22
## eu-central-1 (프랑크푸르트)

# Site Reliability Engineering(SRE) PreLab 예제 - 12번가

- **개요**
  - 본 예제는 GitOps기반 SRE과정을 위한 클라우드 플랫폼 환경구축과 마이크로서비스 전 라이프사이클(분석/설계, 구현, DevOps 툴체인(파이프라인)을 통한 배포, 운영/모니터링)을 커버하도록 구성된 예제입니다. 
  - 분석/설계/구현을 포함한 클라우드 플랫폼기반 운영(컨테이너 오케스트레이션, 모니터링, 로깅, etc) 중심으로 예제 프로젝트는 전개됩니다.
  - PreLab 기간동안 주어진 마이크로서비스 리소스를 사용하거나, 새로운 도메인 모델을 선정 후 설계/구현/배포/운영/모니터링을 적용해도 됩니다.

- **PreLab 진행일정**
  - 분석/설계 : (1일차) 
    - ~~주제 도메인에 대한 분석/설계~~
    - ~~분석/설계 모델에 대한 마이크로서비스 구현~~
    - MSA 아키텍처 구성 및 Cloud Platform 프로비저닝
    - 산출물 : ~~기능 목록서, 이벤트스토밍 모델, 헥사고날 아키텍처,~~ MSA 아키텍처 구성도 
  
  - 배포/운영/모니터링 구현: (2~3일차) 
    - DevOps Toolchain 구축
    - EKS Cluster에 통합 메시징 인프라 구축 및 테스팅
    - 마이크로서비스 오케스트레이션 (오토 스케일아웃, 무정지 배포)
    - 서비스 메쉬 구성 및 마이크로서비스에 SideCar Injection
    - 통합 모니터링, 로깅, 메시징 플랫폼 모니터링 적용
    - 산출물 : 파이프라인 구성도, 수행 증적(Microservice 오케스트레이션, 서비스 메시, 모니터링/로깅)  
 
  - 산출물 리뷰 : (4일차)
    - MSA PaaS Platform 최종 산출물 통합
    - 오후(13:00)부터 최종 산출물을 통한 개별 검토회 진행
   
  - Phase별 산출물
    - MSA PaaS Platform 산출물은 해당 README.md 화일을 활용하거나 free format으로 제출
    - 제출 시, 반드시 소속과 이름을 기재
 
- **체크포인트** 
  - ~~Domain 주제영역 및 시나리오 이해~~
  - ~~이벤트스토밍 모델 이해~~
  - ~~폴리글랏 프로그래밍 코드(동기호출, 비동기 호출) 이해~~
  - MSA 아키텍처 구성
  - Cloud Platform 프로비저닝 
  - DevOps Toolchain 구축 
  - 분산 메시징 인프라 구축 
  - SLA 운영 - 오토 스케일아웃 (Auto Scale-out) 
  - SLA 운영 - 무정지 배포 (Zero downtime Deploy) 
  - Service Mesh 인프라 구축
  - Service Mesh 기반 마이크로서비스 Resilience 적용
  - 마이크로서비스 통합 모니터링
  - 마이크로서비스 통합 로깅
  - 분산 메시징 플랫폼 모니터링

- **PreLab 참고자료**
  - [핵심요소 기술 교재](https://drive.google.com/drive/folders/1atL7qAz4zPLcMiQvW-3PvrU1qmggdziD) (Ctrl + 클릭)
  - [Mini Shopping mall 이벤트스토밍 모델](https://labs.msaez.io/#/storming/C7pO0ZuWtXXxIKenocD9EMPYrxw2/98f0cfeee84ff3ded1a1b00f9cd38ac3) (Ctrl + 클릭) 
  - Microservice Code 리파지토리 (각 Github에 접속후 'fork'를 눌러 복제)
    - [Gateway] : https://github.com/acmexii/gateway
    - [Order] : https://github.com/acmexii/order
    - [Delivery] : https://github.com/acmexii/delivery
    - [Product] : https://github.com/acmexii/product
    
# Table of contents

- [예제 - 음식배달](#---)
  - [서비스 시나리오](#서비스-시나리오)
  - [체크포인트](#체크포인트)
  - [분석/설계](#분석설계)
  - [구현:](#구현)
    - [DDD 의 적용](#ddd-의-적용)
    - [폴리글랏 퍼시스턴스](#폴리글랏-퍼시스턴스)
    - [폴리글랏 프로그래밍](#폴리글랏-프로그래밍)
    - [동기식 호출과 Fallback 처리](#동기식-호출-과-Fallback-처리)
    - [비동기식 호출과 Eventual Consistency](#비동기식-호출과-Eventual-Consistency)
  - [Cloud Services Provisioning](#Cloud-Services-Provisioning)
  - [배포:](#배포)
    - [파이프라인 생성](#파이프라인-생성)  
  - [운영](#운영)
    - ~~[동기식 호출/서킷 브레이킹/장애격리](#동기식호출-서킷브레이킹-장애격리)~~
    - [SLA 준수 - 오토스케일 아웃](#오토스케일-아웃)
    - [SLA 준수 - 무정지 재배포](#무정지-재배포)
    - [Service Mesh 인프라구축](#Service-Mesh)
    - [마이크로서비스 통합 Monitoring](#통합-Monitoring)
    - [마이크로서비스 통합 Logging](#통합-Logging)
    - [이벤트 스트리밍 플랫폼 Monitoring](#이벤트-스트리밍-플랫폼-Install-및-Monitoring)
  - [신규 개발 조직의 추가](#신규-개발-조직의-추가)


# Event Storming
# MSA Architecture
# Cloud Platform 프로비저닝
### EKS Cluster Provisioning
![image](https://user-images.githubusercontent.com/56831448/175239609-d0cdef0c-78be-4ed4-81f1-d72b6ccac823.png)

### EKS Node Provisiong
![image](https://user-images.githubusercontent.com/56831448/175239802-43d0f7cf-6ef1-4099-838b-d6debb420a9b.png)

# DevOps Toolchain 구축
### Codebuild
![image](https://user-images.githubusercontent.com/56831448/175240072-37f6e466-1ac2-4f47-89c8-cadd334add13.png)

### Source GitHub
* gateway (https://github.com/haemyung-yang/gateway)
* order (https://github.com/haemyung-yang/order)
* product (https://github.com/haemyung-yang/product)
* delivery (https://github.com/haemyung-yang/delivery)

### ECR Registry
![image](https://user-images.githubusercontent.com/56831448/175241077-75241e66-3b54-4a85-8948-67a9ed6cc725.png)

# 분산 메시징 인프라 구축


# SLA 운영 - 오토 스케일아웃 (Auto Scale-out) 
# SLA 운영 - 무정지 배포 (Zero downtime Deploy) 
# Service Mesh 인프라 구축
# Service Mesh 기반 마이크로서비스 Resilience 적용
# 마이크로서비스 통합 모니터링
# 마이크로서비스 통합 로깅
# 분산 메시징 플랫폼 모니터링
