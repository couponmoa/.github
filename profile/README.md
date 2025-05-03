![배너 이미지](./images/couponmoa_banner.png)
# 🎟️ 구독 기반 실시간 쿠폰 뷰어 서비스
# 1. 팀원 소개
<table>
  <tr>
    <td width="200" align="center" valign="top">
      <img src="./images/profile_sejin.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>박세진</b><br/>
      <span style="color: orange;">🪪 팀장</span><br/><br/>
      📌 가게 및 쿠폰 구독 기능 구현<br/>
      📌 구독 알림 메일 전송 기능 구현<br/>
      📌 CI/CD 구현<br/>
      📌 Store Service 분리
    </td>
    <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/oh.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>오명제</b><br/>
      <span style="color: red;">🧱 부팀장</span><br/><br/>
      📌 가게 CRUD 구현<br/>
      📌 ElasticSearch<br/>
      📌 AI 추천 기능 구현
    </td>
    <td width="200" align="center" valign="top">
      <img src="./images/profile_nayeon.png" width="100%" style="border-radius:10px;"><br/>
      <b>김나연</b><br/>
      <span style="color: blue;">💡 팀원</span><br/><br/>
      📌 사용자 CRUD 및 인증 / 인가 구현<br/>
      📌 쿠폰 만료 및 발급 알림 구현<br/>
      📌 Gateway 및 User Service 분리<br/>
      📌 Terraform 기반 인프라 구성
    </td>
    <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/kim.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>이보연</b><br/>
      <span style="color: blue;">💡 팀원</span><br/><br/>
      📌 쿠폰 발급 및 발급 내역 관리 기능 구현<br/>
      📌 쿠폰 발급 시 동시성 제어 구현<br/>
      📌 쿠폰 사용량 집계 배치 작업 구현<br/>
      📌 Notification Service 분리
    </td>
        <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/kim.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>권호준</b><br/>
      <span style="color: blue;">💡 팀원</span><br/><br/>
      📌 쿠폰 CRUD 구현<br/>
      📌 모니터링 툴 세팅<br/>
      📌 캐싱, 인덱싱을 적용한 조회 성능 최적화<br/>
      📌 Coupon Service 분리
    </td>
  </tr>
</table>

# 2. 🔁 서비스 플로우

![서비스플로우 이미지](./images/service_flow.png)

# 3. 🔥 주요 기능
👤 **사업자용 기능**
- 가게 등록 / 수정 / 삭제
- 쿠폰 발행
- 쿠폰 사용랑 통계 조회
---
🛍️ **사용자용 기능**

- 매장별 쿠폰 탐색 및 AI 쿠폰 추천
- 쿠폰 발급 및 발급 내역 확인
- 관심 매장 쿠폰 발행 및 쿠폰 만료 알림 기능
---
🧭 **공통 기능**

- 로그인 및 권한 분리 (사업자 / 일반 사용자)
- 회원 프로필 관리
- 쿠폰 상태 자동 변경 (만료 처리 등)

# 4. 🖥️ System Architecture

## 🧩 아키텍처 개요

    **쿠폰모아**는 **MSA (Microservice Architecture)** 기반으로 구성된 웹 서비스입니다.
    기능별로 독립된 서비스를 구성하여 유지보수성과 확장성을 높였으며, Spring Cloud Gateway를 통해 클라이언트 요청을 각 서비스로 라우팅합니다.

---
## 🗺️ AWS 인프라 아키텍처
![인프라 아키텍처 이미지](./images/AWS_infra_architecture.png)

<details>
<summary><strong>📦 아키텍처 구성 요소</strong></summary>

<table>
  <tr>
    <td><b>네트워크</b></td>
    <td>VPC, Public / Private Subnet, Internet Gateway, NAT Gateway</td>
  </tr>
  <tr>
    <td><b>트래픽 라우팅</b></td>
    <td>Route 53, Application Load Balancer (ALB)</td>
  </tr>
  <tr>
    <td><b>컨테이너 오케스트레이션</b></td>
    <td>ECS + Fargate 기반 마이크로서비스<br/>서비스 디스커버리를 위한 Cloud Map</td>
  </tr>
  <tr>
    <td><b>데이터 계층</b></td>
    <td>RDS(MySQL), ElastiCache(Redis), EC2 기반 Elasticsearch</td>
  </tr>
  <tr>
    <td><b>정적 리소스 처리</b></td>
    <td>S3 + CloudFront (이미지 등 정적 자원 처리)</td>
  </tr>
  <tr>
    <td><b>비동기 통신</b></td>
    <td>Amazon SQS (알림 처리용 메시지 큐)</td>
  </tr>
  <tr>
    <td><b>모니터링 / 알림</b></td>
    <td>Prometheus, Grafana, CloudWatch, EventBridge, Lambda</td>
  </tr>
  <tr>
    <td><b>CI / CD 및 이미지 저장소</b></td>
    <td>GitHub Actions → ECR</td>
  </tr>
</table>

</details>

<details>
<summary><strong>🛠️ Terraform 기반 인프라 관리</strong></summary>

### ▪️ Infrastructure as Code (IaC)

- 모든 리소스를 Terraform으로 선언적 관리

### ▪️ 구성 파일 구조

- 기능별 분리:  
  <code>vpc.tf</code>, <code>ecs.tf</code>, <code>alb.tf</code>, <code>sg.tf</code>, <code>rds.tf</code>, <code>sqs.tf</code>, <code>ecr.tf</code> 등

</details>

---

## 🧱 애플리케이션 아키텍처
<table>
  <tr>
    <td><b>서비스명</b></td>
    <td><b>역할</b></td>
  </tr>
  <tr>
    <td><b>Spring Cloud Gateway</b></td>
    <td>클라이언트 요청을 내부 서비스로 라우팅, 인증 및 인가</td>
  </tr>
  <tr>
    <td><b>User Service</b></td>
    <td>회원가입, 로그인, 사용자 정보 관리</td>
  </tr>
  <tr>
    <td><b>Store Service</b></td>
    <td>매장 생성, 수정, 조회</td>
  </tr>
  <tr>
    <td><b>Coupon Service</b></td>
    <td>쿠폰 발행, 발급, 조회, 사용</td>
  </tr>
  <tr>
    <td><b>Notification Service</b></td>
    <td>쿠폰 생성, 발급, 만료 알림 전송</td>
  </tr>
  <tr>
    <td><b>Batch Service</b></td>
    <td>쿠폰 사용량 집계 등 배치 작업</td>
  </tr>
  <tr>
    <td><b>AI Service</b></td>
    <td>AI 기반 추천 쿠폰 ID 생성</td>
  </tr>
</table>

<details>
<summary><strong>서비스 간 통신 방식</strong></summary>

서비스 간 통신은 **gRPC** 또는 **비동기 메시지 큐(AWS SQS)**를 통해 이루어집니다.

- **동기 처리**가 필요한 경우 성능이 뛰어난 **gRPC** 방식으로 서비스를 호출하고,
- **비동기 처리**가 필요한 이벤트성 작업은 **SQS Queue**를 통해 안정적으로 전달됩니다.

**📶 예시**
<table>
  <tr>
    <td><b>통신 주체</b></td>
    <td><b>방식</b></td>
    <td><b>설명</b></td>
  </tr>
  <tr>
    <td>Coupon Service → Store Service</td>
    <td>gRPC</td>
    <td>쿠폰 생성 시 매장 유효성 확인</td>
  </tr>
  <tr>
    <td>Coupon Service → Notification Service</td>
    <td>SQS</td>
    <td>쿠폰 발급 완료 후 알림 전송</td>
  </tr>
</table>

</details>

---

## 🔄 GitHub Actions를 활용한 CI/CD Pipeline

<div style="border:1px solid #ddd; border-radius:6px; padding:12px; margin:10px 0;">

### ✅ CI  
- <b>Trigger:</b> dev 브랜치에 코드가 push되거나 PR 발생 시 실행  
- <b>Test:</b> 테스트 코드 수행  

</div>

<div style="border:1px solid #ddd; border-radius:6px; padding:12px; margin:10px 0;">

### ✈️ CD  
- <b>Trigger:</b> dev 브랜치에 코드가 push되고, CI가 성공적으로 완료되었을 때 실행  

- <b>Build & Deploy:</b>  
  • AWS ECR에 Docker 이미지 빌드 및 푸시  
  • AWS ECS에 새로운 이미지로 서비스 업데이트  

- <b>Security:</b> GitHub Secrets에 AWS 인증 정보 등 비밀 값 저장  

</div>

# 5. 💡 ERD설계

<details>
  <summary><strong>v1(초반 설계)</strong></summary>
  <img src="./images/ERD_v1.png">
</details>

<details>
  <summary><strong>v2(MSA 설계 전)</strong></summary>
  <img src="./images/ERD_v2.png">
</details>

<details>
  <summary><strong>v3 (MSA 설계 후)</strong></summary><br/>
  
  ### 📌 user service  
  <img src="./images/ERD_v3_user.png" width="300">

  ### 📍 store service  
  <img src="./images/ERD_v3_store.png" width="300">

  ### 🎟️ coupon service  
  <img src="./images/ERD_v3_coupon.png" width="300">

  ### ✉️ notification service  
  <img src="./images/ERD_v3_notification.png" width="300">

</details>

---

# 6 🧐 기술적 의사 결정

<details>
  <summary>🏃 [비동기 처리] 쿠폰 만료 알림 비동기 처리</summary>
</details>

<details>
  <summary>🏃 [동시성 제어] 쿠폰 발급 시 동시성 제어 전략 비교</summary>
</details>

<details>
  <summary>🏃 [비동기 처리] 구독 시 신규 쿠폰 알림 비동기 처리</summary>
</details>

<details>
  <summary>🏃 [AI 활용] AI를 활용한 개인화 쿠폰 추천 시스템</summary>
</details>

<details>
  <summary>🏃 [Architecture] MSA 구조화 및 배포</summary>
</details>

<details>
  <summary>🏃 [API 문서화] Spring REST Docs 도입</summary>
</details>

<details>
  <summary>🏃 [인프라 전략] IaC 기반 클라우드 인프라 구성 배경</summary>
</details>

---

# 7. ⚡ 성능 비교

<details>
  <summary>🏃 [WebFlux] SSE 알림에서 MVC와 WebFlux 방식 성능 비교</summary>
</details>

<details>
  <summary>🏃 [CloudFront] 이미지 조회 시 S3, CloudFront 성능 비교</summary>
</details>

<details>
  <summary>🏃 [Spring Batch] 쿠폰 사용량 집계 작업의 성능 개선기</summary>
</details>

<details>
  <summary>🏃 [gRPC] 서버간 gRPC vs REST 통신 방식 성능 비교 </summary>
</details>

<details>
  <summary>🏃 [ElasticSearch] 쿠폰 검색 기능 DB, ElasticSearch 성능 비교</summary>
</details>

---

# 8. 🚨 트러블슈팅

<details>
  <summary>🎯 Spring 애플리케이션에서 Fargate Task Role 인식 실패 문제 해결</summary>
</details>

<details>
  <summary>🎯 ECS 배포 시 오토스케일링 문제</summary>
</details>

---

# 9. 🛠 기술 스택

<table width="100%">
  <tr>
    <td valign="top">
      <b>🧠 Language</b><br/>
        <img src="https://github.com/user-attachments/assets/7f579478-f27c-4218-882b-4eb8bc302b32" width="20">
        Java<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>👨‍💻 Backend</b><br/>
        <img src="https://github.com/user-attachments/assets/9c525a7b-97fa-496e-b6f0-76406da17364" width="20"/>
        Spring<br/>
        <img src="https://github.com/user-attachments/assets/d903bb42-e1e5-4221-a0a4-6f4bd5b30edd" width="20">
        Spring Boot<br/>
        <img src="https://github.com/user-attachments/assets/d903bb42-e1e5-4221-a0a4-6f4bd5b30edd" width="20">
        Spring Data JPA<br/>
        <img src="https://github.com/user-attachments/assets/6b6bf960-df68-4fac-ba6c-2f932b102fb0" width="20">
        Spring WebFlux<br/>
        <img src="https://github.com/user-attachments/assets/074a6663-6d96-4e39-99cd-db9cf69a8f08" width="20">
        QueryDSL<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>💾 Database</b><br/>
        <img src="https://github.com/user-attachments/assets/60d30203-d078-4c8e-bb2f-ce5ef7f0c8f5" width="20">
        MySQL<br/>
        <img src="https://github.com/user-attachments/assets/68df185a-c4bd-42e0-b13d-dc8b2a85fb79" width="20">
        Redis<br/>
        <img src="https://github.com/user-attachments/assets/dce85062-cc7f-417b-8c65-68a33b67197a" width="20">
        Elasticsearch<br/>
        <img src="https://github.com/user-attachments/assets/a35ff5bf-627e-41cf-9493-cc2f6363a255" width="20">
        AWS ElastiCache<br/>
        <img src="https://github.com/user-attachments/assets/06b5d7ad-e6af-4c21-ac05-3b8ea44427fb" width="20">
        AWS RDS<br/><br/>
  <hr style="margin:4px 0;"/>
      <b>📧 Email</b><br/>
        <img src="https://github.com/user-attachments/assets/c3a24ead-982e-443c-affa-8d5b3f10a6f4" width="20">
        JavaMailSender<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>📈 Monitoring</b><br/>
        <img src="https://github.com/user-attachments/assets/592a5e38-ada1-4179-89f3-7827f2008b5d" width="20">
        Spring Actuator<br/>
        <img src="https://github.com/user-attachments/assets/47d7f66a-5d55-44a4-b4b4-e167ba3982fb" width="20">
        Prometheus<br/>
        <img src="https://github.com/user-attachments/assets/15babcd1-8774-4ec3-b996-c774eff08006" width="20">
        Grafana<br/>
        <img src="https://github.com/user-attachments/assets/8f67e9eb-0f56-4717-8606-ad6559fc284a" width="20">
        AWS CloudWatch<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>👥 Collaboration</b><br/>
        <img src="https://github.com/user-attachments/assets/4c2545a7-e860-4633-98c9-0f21de57b642" width="20">
        GitHub<br/>
        <img src="https://github.com/user-attachments/assets/ab197765-c786-4b53-89eb-bb568604f9b1" width="20">
        Slack<br/>
        <img src="https://github.com/user-attachments/assets/37046169-d290-4cb6-b0a2-4cc95f033fb6" width="20">
        Notion<br/>
        <img src="https://github.com/user-attachments/assets/039d9816-7763-48b6-ba1b-443b3a1bfa62" width="20">
        ERD Cloud<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>🏁 Test</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Spring REST Docs<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Postman<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Swagger<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        JMeter
        <img src="https://github.com/user-attachments/assets/" width="20">
        K6<br/><br/>
      <hr style="margin:4px 0;"/>
    </td>
    <td valign="top">
      <b>🌐 IDE</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        IntelliJ IDEA<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>🔐 Security</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        JWT<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Spring Security<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>⏰ Batch</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Spring Batch<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>✉️ Inter-Service Communication</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        AWS SQS<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        gRPC<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>🤖 AI</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Google Gemini API<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Spring AI<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>✈️ CI/CD</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        GitHub Actions<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Docker<br/><br/>
      <hr style="margin:4px 0;"/>
      <b>☁️ Infra</b><br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Terraform<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        AWS ECS, ECR, Fargate, EC2<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        AWS Route53, S3, ALB<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Internet Gateway, NAT Gateway<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        AWS IAM, Lambda, EventBridge<br/>
        <img src="https://github.com/user-attachments/assets/" width="20">
        Cloud Map, Certificate Manager<br/><br/>
      <hr style="margin:4px 0;"/>
    </td>
  </tr>
</table>
