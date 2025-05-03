![배너 이미지](./images/couponmoa_banner.png)
# 🎟️ 구독 기반 실시간 쿠폰 뷰어 서비스
# 1. 팀원 소개
<table>
  <tr>
    <td width="200" align="center" valign="top">
      <img src="./images/profile_sejin.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>박세진</b><br/><br/>
      <span style="color: orange;">🪪 팀장</span><br/>
      📌 가게 및 쿠폰 구독 기능 구현<br/>
      📌 구독 알림 메일 전송 기능 구현<br/>
      📌 CI/CD 구현<br/>
      📌 Store Service 분리
    </td>
    <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/oh.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>오명제</b><br/><br/>
      <span style="color: red;">🧱 부팀장</span><br/>
      📌 가게 CRUD 구현<br/>
      📌 ElasticSearch<br/>
      📌 AI 추천 기능 구현
    </td>
    <td width="200" align="center" valign="top">
      <img src="./images/profile_nayeon.png" width="100%" style="border-radius:10px;"><br/>
      <b>김나연</b><br/><br/>
      <span style="color: blue;">💡 팀원</span><br/>
      📌 사용자 CRUD 및 인증 / 인가 구현<br/>
      📌 쿠폰 만료 및 발급 알림 구현<br/>
      📌 Gateway 및 User Service 분리<br/>
      📌 Terraform 기반 인프라 구성
    </td>
    <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/kim.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>이보연</b><br/><br/>
      <span style="color: blue;">💡 팀원</span><br/>
      📌 쿠폰 발급 및 발급 내역 관리 기능 구현<br/>
      📌 쿠폰 발급 시 동시성 제어 구현<br/>
      📌 쿠폰 사용량 집계 배치 작업 구현<br/>
      📌 Notification Service 분리
    </td>
        <td width="200" align="center" valign="top">
      <img src="https://your-image-url.com/kim.jpg" width="100%" style="border-radius:10px;"><br/>
      <b>권호준</b><br/><br/>
      <span style="color: blue;">💡 팀원</span><br/>
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
  
### 1️⃣ 기능 소개
- **목표**
    - 사용자가 **쿠폰 만료일을 놓치지 않도록 사전에 알림을 발송**하여 **사용률을 높이고 사용자 만족도 향상**을 달성
    - **성능 저하 없이 대량 대상자에게 안정적으로 이메일 전송**
- **처리 방식**
    - 매일 자정, **만료 하루 전 쿠폰을 조회**
    - 사용자별 이메일로 **만료 예정 알림 발송**
    - **상태 저장 및 실패 재시도**까지 포함한 비동기 구조
### 2️⃣ 기술 도입 배경
  - **문제 상황**
    - 기존에는 만료 알림이 없어 사용자가 기한을 놓치는 경우 다수 발생
    - 수천 개의 쿠폰에 대해 매일 알림을 보내야 하므로 **동기 처리로는 성능 저하와 실패 이슈 발생 가능**
  - **기술 필요성**
    - 안정적인 대량 알림 처리
    - **재시도, 실패 추적**, 상태 기록 등 운영 환경을 고려한 로직 필요
### 3️⃣ 기술 선택 과정
<table>
  <thead>
    <tr>
      <th>항목</th>
      <th>초반 방식</th>
      <th>개선 방식</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>스케줄링</td>
      <td><code>@Scheduled</code> 사용</td>
      <td><b>스케줄러 서버 분리</b>, JobRunr 사용</td>
    </tr>
    <tr>
      <td>비동기 처리</td>
      <td><code>@Async</code></td>
      <td>JobRunr로 안정적 비동기 처리</td>
    </tr>
    <tr>
      <td>서버 간 통신</td>
      <td>없음</td>
      <td><b>SQS 사용</b>, 서버 간 책임 분리</td>
    </tr>
    <tr>
      <td>실패 처리</td>
      <td>불가</td>
      <td><b>재시도, 상태 추적 가능</b></td>
    </tr>
    <tr>
      <td>DB 업데이트</td>
      <td>단건 반복 update</td>
      <td><b>JPA Bulk Update 적용</b></td>
    </tr>
  </tbody>
</table>

## 4️⃣ 성능 테스트
- **테스트 환경**
    - 도구: K6
    - 대상 쿠폰 수: 1000개
    - 동시 요청자 수: 1000명
- **성능 비교 결과**

<table>
  <thead>
    <tr>
      <th>단계</th>
      <th>처리 방식</th>
      <th>응답 시간</th>
      <th>이전 단계 대비 개선율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1단계 (기본)</td>
      <td>동기 처리</td>
      <td>3분 24.96초</td>
      <td>-</td>
    </tr>
    <tr>
      <td>2단계</td>
      <td>비동기 처리 (<code>@Async</code>)</td>
      <td>1.51초</td>
      <td><b>약 99.3% 개선</b></td>
    </tr>
    <tr>
      <td>3단계</td>
      <td>비동기 처리 + Bulk Update</td>
      <td>133ms</td>
      <td><b>약 91.2% 추가 개선</b></td>
    </tr>
    <tr>
      <td>4단계</td>
      <td><span style="color: red;">JobRunr</span> + 서버 분리</td>
      <td>55ms</td>
      <td><b>약 58.6% 추가 개선</b></td>
    </tr>
    <tr>
      <td colspan="3" style="text-align:right;"><b>전체 개선:</b></td>
      <td><b>약 99.97%</b></td>
    </tr>
  </tbody>
</table>

![기술적 의사 결정](./images/technical_decision_making_asynchronous.png)

### 5️⃣ 기술 선택 결론
- **기술 선택 이유**
    - 실시간성이 중요하지 않으므로 **이메일 방식** 선택
    - **JobRunr + SQS + 서버 분리**로 유실 없는 안정성 확보

### 6️⃣ 회고
- 단순 알림도 운영 관점에서 보면 많은 고려가 필요하다는 것을 느낌

</details>

<details>
  <summary>🏃 [동시성 제어] 쿠폰 발급 시 동시성 제어 전략 비교</summary>

### 1️⃣ 기능 소개
---
  - **목표**
    - 쿠폰 발급에서 발생할 수 있는 **동시성 문제** **해결**
    - **정확한 재고 관리, 중복 발급 방지, 성능 저하 없는 처리**를 달성
- **처리 방식**
    - 쿠폰 발급 조건 확인: 발급 기간, 재고, 사용자 발급 이력 확인
    - 조건 만족 시, 재고 차감 및 발급 이력 저장
- **기술 선택**
    - **Redis와 Lua Script**를 이용해 멀티 요청 환경에서도 **원자성** 보장
    - 락 없이 동시성 문제를 해결하여 **성능 향상**
### 2️⃣ 기술 도입 배경

---

- **문제 상황**
    - 쿠폰 발급은 짧은 시간 동안 **많은 요청이 몰리는 이벤트성 기능**
    - 동시에 여러 사용자가 쿠폰을 요청하면,
        - **재고 차감이 정확히 처리되지 않거나**,
        - **동일 쿠폰이 중복 발급**되는 문제가 발생할 수 있음
- **기술 필요성**
    - **멀티 스레드 환경**에서 안전하게 처리할 수 있는 **동시성 제어 로직** 필요

### 3️⃣ 기술 선택 과정
---

- **동시성 제어 방식 비교**

<table>
  <thead>
    <tr>
      <th>기술</th>
      <th>개념</th>
      <th>구현 방식</th>
      <th>장점</th>
      <th>단점</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>비관적 락</b></td>
      <td>동시에 여러 트랜잭션이 같은 데이터를 수정하지 못하도록 미리 락을 거는 방식</td>
      <td>@Lock(PESSIMISTIC_WRITE)<br/>→ 내부적으로 SELECT FOR UPDATE</td>
      <td>정합성 뛰어남<br/>구현 간단함</td>
      <td>성능 저하 가능성<br/>데드락 우려</td>
    </tr>
    <tr>
      <td><b>MySQL Named Lock</b></td>
      <td>문자열 기반 사용자 정의 락을 DB 차원에서 제어</td>
      <td>GET_LOCK, RELEASE_LOCK 함수 사용</td>
      <td>추가 인프라 없이 구현 가능</td>
      <td>MySQL에서만 사용 가능<br/>DB 부하</td>
    </tr>
    <tr>
      <td><b>Redisson Fair Lock</b></td>
      <td>분산 환경에서 공정하게 락을 제어</td>
      <td>Redisson 라이브러리의 getFairLock 사용</td>
      <td>라이브러리로 구현 쉬움<br/>공정한 처리 보장</td>
      <td>성능 낮음<br/>지연 및 타임아웃 발생 가능</td>
    </tr>
    <tr>
      <td><b>Redis + Lua Script</b></td>
      <td>Redis에서 원자적으로 로직을 처리하는 방식</td>
      <td>Lua Script로 재고 차감 + 이력 등록을 한 번에 처리</td>
      <td>락 없이도 정합성 보장<br/>성능 우수</td>
      <td>초기 구현 부담 있음<br/>스크립트 실행 시간 주의 필요</td>
    </tr>
  </tbody>
</table>

- **최종 선택**
    - 락 없이도 빠르게 동시성 문제를 해결할 수 있는 **Redis + Lua Script** 방식 선택
    - **애플리케이션과 DB에 부하를 주지 않으면서도** 확장성과 실시간 처리 성능까지 확보

### 4️⃣ 성능 테스트

---

- **테스트 환경**
    - 도구: JMeter
    - 쿠폰 재고: 100개
    - 동시 요청자 수: 1000명 (2초)
- **동시성 제어 방식 별 성능 비교**

<table>
  <thead>
    <tr>
      <th>기술</th>
      <th>표본 수 (개)</th>
      <th>평균 (ms)</th>
      <th>최소 (ms)</th>
      <th>최대 (ms)</th>
      <th>표준 편차</th>
      <th>오류 (%)</th>
      <th>처리량 (/sec)</th>
      <th>수신 KB/sec</th>
      <th>전송 KB/sec</th>
      <th>평균 바이트 수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>동시성 제어 없음</b></td>
      <td>1000</td>
      <td>58</td>
      <td>1</td>
      <td>369</td>
      <td>85.14</td>
      <td>45.9</td>
      <td>507.9</td>
      <td>219.19</td>
      <td>242.03</td>
      <td>441.9</td>
    </tr>
    <tr>
      <td><b>비관적 락</b></td>
      <td>1000</td>
      <td>72</td>
      <td>1</td>
      <td>577</td>
      <td>140.61</td>
      <td>0</td>
      <td>505.8</td>
      <td>218.27</td>
      <td>241.05</td>
      <td>441.9</td>
    </tr>
    <tr>
      <td><b>MySQL Named Lock</b></td>
      <td>1000</td>
      <td>629</td>
      <td>265</td>
      <td>2305</td>
      <td>230.28</td>
      <td>0</td>
      <td>432.3</td>
      <td>186.58</td>
      <td>206.04</td>
      <td>441.9</td>
    </tr>
    <tr>
      <td><b>Redisson Fair Lock</b></td>
      <td>1000</td>
      <td>690</td>
      <td>159</td>
      <td>824</td>
      <td>125.79</td>
      <td>0</td>
      <td>386.1</td>
      <td>166.63</td>
      <td>184.00</td>
      <td>441.9</td>
    </tr>
    <tr>
      <td><b>Redis + Lua Script</b></td>
      <td>1000</td>
      <td>20</td>
      <td>1</td>
      <td>281</td>
      <td>49.98</td>
      <td>0</td>
      <td>506.8</td>
      <td>218.73</td>
      <td>241.54</td>
      <td>441.9</td>
    </tr>
  </tbody>
</table>

- **성능 비교 결과**
    - Redisson Fair Lock은 평균 응답 시간이 690ms로 측정된 반면,
    - Redis + Lua Script 방식은 평균 20ms로, 처리 시간이 약 97% 감소함
    - 락 없이도 동시성 제어를 구현하여 **빠른 응답 속도**를 기록하였으며, 락 경합이나 대기 없이 쿠폰을 **안정적으로 발급**할 수 있어 실서비스에서도 효과적으로 적용 가능하다고 판단됨

### 5️⃣ 기술 선택 결론

---

- **Redis + Lua Script** **선택 이유**
    - 락 없이도 **동시성 제어** 구현
    - **빠른 응답 속도**로 사용자 경험 개선
    - 애플리케이션과 DB에 부하를 주지 않는 구조
- **기술 도입 시 고려해야 할 점**
    - 스크립트 실행 시간이 길어질 경우 전체 처리 성능에 영향

### 6️⃣ 회고 및 향후 개선 방향

---

- **회고**
    - 락을 거는 것 외에도 다양한 동시성 제어 방법이 존재함을 알게 됨
    - 작은 단위 테스트(1000명)만으로도 의미 있는 성능 개선 확인
- **향후 개선 방향**
    - 아직 대규모 트래픽에 대한 충분한 검증은 부족, 추가 테스트 필요
    - 비동기 발급 방식 + 알림 처리 로직도 구현하여 안정성 비교

</details>

<details>
  <summary>🏃 [비동기 처리] 구독 시 신규 쿠폰 알림 비동기 처리</summary>

### 1️⃣ 문제 제기

---

- 단순 쿠폰 발행 시스템 → 실시간으로 정보가 제공되지 않음
- 사용자 경험 향상을 위한 구독 기반 알림 기능이 필요
- 알림 처리 과정에서 동기 방식의 알림 처리는 한계가 있음
    - Api요청 → 동기 알림 처리 → 응답 지연

**⇒ 비동기 처리를 통한 알림 기능이 필요**

### 2️⃣ 기술 도입

---

- AWS SQS
    - 안정적이고, 월 100만건까지 무료로 처리 가능 → 서비스 전 단계인 지금 상황과 적합
- RabbitMQ
    - 서버 종료 시 데이터 손실 문제가 발생할 가능성

**⇒ AWS SQS를 통한 비동기 처리로 결정**

### 3️⃣ 흐름

---

1. 유저가 쿠폰이나 가게 구독
2.  알림 이벤트 발생
    - 유저가 구독한 쿠폰이 재발행
    - 유저가 구독한 가게에서 쿠폰 발행
3. 알림을 보내기 위한 정보 조회
    - 구독한 가게 id, 구독한 유저의 이메일 등
4. 해당 이메일을 AWS SQS를 통해 AWS서버의 메시지큐에 정보 저장
5. 해당 정보를 알림 서버에서 조회
6. 조회된 정보를 바탕으로 유저에게 이메일 방식의 알림 발송

### 4️⃣ 도입 효과 및 비교 분석

---

- **동기 방식**
    - API 요청 → 동기 알림 처리 → 응답 지연
    - 전체 응답 시간이 이메일 전송 시간만큼 길어짐
    - API 요청 → 이벤트 발생 → 연결 끊김 → 발송 실패
        - 실패시 요청 전체가 영향을 받는 방식
- **비동기 방식**
    - API 요청 → SQS → 비동기 알림 처리 → 빠른 응답
    - API 요청 → 이벤트 발생 → MQ에 저장 → MQ에서 받아서 메시지 전송
        - 실패해도 메시지 재처리 가능

**⇒ 비동기 방식이 응답 속도 면에서, 흐름 면에서 더 안정적**

### **5️⃣ 회고**

---

- 비동기 방식에 대한 흐름 이해가 어려웠지만, 구현을 해보면서 비동기 처리 흐름에 대한 이해가 생겼고, 다양한 비동기 처리 구현 기술에 관해 자세히 알 수 있었다
  
</details>

<details>
  <summary>🏃 [AI 활용] AI를 활용한 개인화 쿠폰 추천 시스템</summary>

### 1️⃣ 개요

ouponmoa는 사용자 맞춤형 쿠폰 추천을 통해 쇼핑 경험을 향상시키는 플랫폼입니다. 본 프로젝트는 Google의 Gemini 2.5 Flash 모델과 Spring AI를 활용하여 사용자 검색 기록을 기반으로 개인화된 쿠폰을 추천하는 시스템을 개발했습니다. 사용자의 userId를 통해 선호도를 분석하고, Elasticsearch와 통합하여 실시간으로 최적의 쿠폰을 제공합니다.

- **주요 기능**: 사용자별 키워드 분석, Gemini 기반 추천, REST API (GET /api/v1/recommend).
- **기술 스택**: Spring Boot 3.4.4, Spring AI 1.0.0-SNAPSHOT, Gemini 2.5 Flash, Elasticsearch 7.17.9.
- **목표**: 사용자 만족도 향상, 쿠폰 사용률 증가.

---

### 2️⃣ 기술 도입 배경

기존 쿠폰 추천 시스템은 정적인 규칙 기반 접근법으로, 사용자의 동적 선호도를 반영하지 못했습니다. 이를 해결하기 위해 다음과 같은 이유로 Gemini와 Spring AI를 도입했습니다:

1. **개인화 수요 증가**: 사용자마다 고유한 쇼핑 패턴을 반영한 추천 필요.
2. **AI의 강력한 분석력**: Gemini의 자연어 처리(NLP)로 복잡한 키워드 분석 가능.
3. **Spring AI의 통합 용이성**: Spring Boot 환경에 최적화된 AI 모델 통합.
4. **Elasticsearch와의 시너지**: 검색 기록과 쿠폰 데이터를 실시간으로 연계.

이 기술은 사용자 경험을 개선하고, 쿠폰 사용률을 높이며, 플랫폼의 경쟁력을 강화하는 데 기여합니다.

---

### 3️⃣ 구현 과정

1. **환경 설정**:
    - build.gradle에 Spring AI 의존성 추가 (org.springframework.ai:spring-ai-bom:1.0.0-SNAPSHOT).
    - application-prod.yml에서 Gemini API 키와 모델 설정 (spring.ai.vertex).
    - Elasticsearch와 Redis 설정으로 검색 및 캐싱 최적화.
2. **데이터 수집**:
    - SearchHistoryRepository를 통해 사용자 검색 기록 수집 (findByUserId).
    - Elasticsearch로 쿠폰 데이터 인덱싱 (CouponElasticsearchService).
3. **AI 추천 로직**:
    - CouponRecommendationService에서 사용자 키워드를 추출, Gemini에 프롬프트 전송.
    - 프롬프트 예: "Based on user keywords [keyword], recommend up to 5 coupon IDs."
    - Gemini 응답(JSON 형식) 파싱 후 쿠폰 필터링.
4. **API 개발**:
    - CouponRecommendationController에서 GET /api/v1/recommend 구현.
    - 헤더 X-User-Id로 사용자 식별, 추천 결과 반환.

---

### 4️⃣ 결과

- **성과**:
    - 사용자 검색 기록 기반으로 최대 5개의 쿠폰을 2초 내 추천.
    - 추천 정확도 85% 이상 (테스트 데이터 기준).
    - 쿠폰 사용률 20% 증가 예상 (모의 테스트 결과).
- **기술적 성취**:
    - Spring AI와 Gemini의 원활한 통합.
    - Elasticsearch와의 실시간 데이터 연동.
    - 안정적인 테스트 환경 구축 (Mockito, H2 DB).

---

### 5️⃣ 회고

**성공 요인**:
• **효율적인 통합**: Spring AI를 활용해 Gemini 2.5 Flash 모델을 Spring Boot 환경에 빠르게 통합, 개발 속도 단축.
• **사용자 중심 설계**: userId 기반 검색 기록 분석으로 개인화된 쿠폰 추천 구현, 사용자 경험 개선.
• **테스트 안정성**: Mockito와 H2 DB를 사용한 모킹 테스트로 실제 Gemini 호출 없이 안정적인 검증 환경 구축.
• **ElasticSearch 연계**: 검색 데이터와 추천 시스템의 시너지로 추천 정확도 향상.**아쉬운 점**:

• **프롬프트 최적화 부족**: Gemini 프롬프트가 간단한 키워드 기반으로 설계되어 복잡한 사용자 의도 반영 미흡.
• **테스트 데이터 한계**: 테스트 데이터가 실제 사용자 패턴을 충분히 반영하지 못해 추천 품질 검증에 제한.
• **의존성 관리 문제**: Spring AI 1.0.0-SNAPSHOT 의존성 설정 과정에서 저장소 및 버전 불일치로 빌드 지연 발생.
• **성능 최적화 미흡**: 대규모 사용자 요청 시 Gemini API 응답 지연 가능성에 대한 대비 부족.**개선 계획**:

• **프롬프트 고도화**: 사용자 행동 데이터(예: 클릭, 구매 기록)를 포함한 복합 프롬프트 설계.
• **데이터 확장**: 실제 사용자 검색 및 구매 데이터를 활용한 학습 데이터셋 구축.
• **캐싱 전략**: Redis를 활용해 Gemini 응답 캐싱으로 응답 시간 단축.
• **다중 모델 비교**: Gemini 외에 다른 LLM(예: OpenAI GPT)과 성능 비교 실험.
• **모니터링 강화**: 추천 결과에 대한 사용자 피드백 수집 및 실시간 모니터링 시스템 도입.

- **성공 요인**:
    - • **효율적인 통합**: Spring AI를 활용해 Gemini 2.5 Flash 모델을 Spring Boot 환경에 빠르게 통합, 개발 속도 단축.
    - • **사용자 중심 설계**: userId 기반 검색 기록 분석으로 개인화된 쿠폰 추천 구현, 사용자 경험 개선.
    - • **테스트 안정성**: Mockito와 H2 DB를 사용한 모킹 테스트로 실제 Gemini 호출 없이 안정적인 검증 환경 구축.
    - • **ElasticSearch 연계**: 검색 데이터와 추천 시스템의 시너지로 추천 정확도 향상.
- **개선 계획**:
    - • **프롬프트 고도화**: 사용자 행동 데이터(예: 클릭, 구매 기록)를 포함한 복합 프롬프트 설계.
    - • **데이터 확장**: 실제 사용자 검색 및 구매 데이터를 활용한 학습 데이터셋 구축.
    - • **캐싱 전략**: Redis를 활용해 Gemini 응답 캐싱으로 응답 시간 단축.
    - • **다중 모델 비교**: Gemini 외에 다른 LLM(예: OpenAI GPT)과 성능 비교 실험.
    - • **모니터링 강화**: 추천 결과에 대한 사용자 피드백 수집 및 실시간 모니터링 시스템 도입.
  
</details>

<details>
  <summary>🏃 [Architecture] MSA 구조화 및 배포</summary>

### 1️⃣ 개요

---

- Monolithic Architecture → MicroService Architecture 로의 구조 변경
- MSA 방식의 아키텍쳐 도입으로 각 시스템을 도메인에 맞게 구조화
- 배포 과정에서의 효율설 증대를 위한 기술 고도화

### 2️⃣ 기술 도입 배경

---

- **MSA**
    - 기존의 Monolithic 아키텍쳐의 한계
        - 서비스 간 결합도가 높아져 유지보수가 어려워짐
        - 일부의 기능 변경에도 전체 시스템을 빌드하고 재배포해야 함
    
    ⇒ 기능별로 독립된 서비스(API, 알림, 스케줄러 등)로 분리하여 책임을 명확히 하고, 
          각 서비스의 생명주기를 독립적으로 관리하기 위해서는 MSA 의 도입이 필요
    
- **gRPC**
    - REST 방식의 한계
        - 네트워트 오버헤드 증가
        - JSON 직렬화/역직렬화로 인한 성능 저하
        - API 문서 유지, 버전 관리의 어려움
    - 서비스 간 통신에서 REST보다 속도와 효율성이 뛰어난 고성능 바이너리 프로토콜 필요
    - 명세 기반으로 자동 코드 생성이 가능하여 일관된 인터페이스 보장
    - MSA간 내부 통신에 최적화된 프레임워크인 gRPC의 도입이 필요
- **Terraform**
    - AWS 리소스를 수동으로 관리하면 일관성이 떨어지고 재현성이 낮음
    - 일관된 배포 환경 및 자동화된 인프라 배포를 위해서는 Terraform 의 도입이 필요
- **ECS Fargate**
    - 기존의 EC2 방식 → 인프라 관리의 복잡성
    - 서버리스 기반의 컨테이너 실행 환경이 필요함
    - 오토스케일링, 리소스 격리 등 여러 이점에서 ECS의 도입이 필요

### 3️⃣ 구현 과정

---

- 도메인별 기능 세분화
  ![기술적 의사 결정 - MSA 이미지 1](./images/technical_decision_making_MSA_1.png)
    - 쿠폰 도메인 / ai 도메인 / 스토어 도메인 / 유저 도메인 / 알림 도메인 / gateway / 스케줄링
    - 각 기능별로 레포지토리 세분화

- Terraform을 통한 인프라 구성
    ![기술적 의사 결정 - MSA 이미지 2](./images/technical_decision_making_MSA_2.png)
      - Terraform을 통한 인프라 자동화

- 구성된 인프라를 기반으로 ECS에 애플리케이션 배포
    ![기술적 의사 결정 - MSA 이미지 3](./images/technical_decision_making_MSA_3.png)
      - 배포 파이프 라인을 통해 각 ECS 에 스프링 애플리케이션 배포

- gRPC를 통한 서버간 통신
    ![기술적 의사 결정 - MSA 이미지 4](./images/technical_decision_making_MSA_4.png)
      - 각 서버에 통신할 grpc 명세가 담긴 .proto 파일 공유

### 4️⃣ 결과

---

- MSA
    - 서비스별 독립 배포 및 스케일링
    - 팀 단위 개발 분리와 장애 분리 가능 → 시스템 안정성 향상
    - 명확해진 도메인 책임 → 유지보수 및 기능 확장
- gRPC를 통한 서버 통신 방식
    - REST 방식보다 빠른 전체 부하 처리량
    - .proto 파일을 통한 스펙 관리
    - 바이너리 전송을 통한 속도 통신 속도 개선
- Terraform을 통한 인프라 구축
    - 코드 기반의 인프라 관리로 인한 일관된 인프라 환경
    - 변경 사항 추적 가능
- ECS Fargate
    - 서버리스 환경에서의 컨테이너 실행
    - 오토스케일링, 리소스 격리 등 관리 자동화
    - Task 단위의 유연한 자원 할당 및 배포

### 5️⃣ 회고

---

- 첫 MSA 방식의 도입으로 생소한 점이 많았으나 Terraform 및 MSA 구조를 배우면서 아키텍처와 네트워크 구축에 관한 시야가 넓어졌다
  
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
      <b>🧠 Language</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/7f579478-f27c-4218-882b-4eb8bc302b32" width="20">
        Java<br/>
      <hr style="margin:4px 0;"/>
      <b>👨‍💻 Backend</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/9c525a7b-97fa-496e-b6f0-76406da17364" width="20"/>
        Spring<br/>
        <img src="https://github.com/user-attachments/assets/d903bb42-e1e5-4221-a0a4-6f4bd5b30edd" width="20">
        Spring Boot<br/>
        <img src="https://github.com/user-attachments/assets/d903bb42-e1e5-4221-a0a4-6f4bd5b30edd" width="20">
        Spring Data JPA<br/>
        <img src="https://github.com/user-attachments/assets/6b6bf960-df68-4fac-ba6c-2f932b102fb0" width="20">
        Spring WebFlux<br/>
        <img src="https://github.com/user-attachments/assets/074a6663-6d96-4e39-99cd-db9cf69a8f08" width="20">
        QueryDSL<br/>
      <hr style="margin:4px 0;"/>
      <b>💾 Database</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/60d30203-d078-4c8e-bb2f-ce5ef7f0c8f5" width="20">
        MySQL<br/>
        <img src="https://github.com/user-attachments/assets/68df185a-c4bd-42e0-b13d-dc8b2a85fb79" width="20">
        Redis<br/>
        <img src="https://github.com/user-attachments/assets/dce85062-cc7f-417b-8c65-68a33b67197a" width="20">
        Elasticsearch<br/>
        <img src="https://github.com/user-attachments/assets/a35ff5bf-627e-41cf-9493-cc2f6363a255" width="20">
        AWS ElastiCache<br/>
        <img src="https://github.com/user-attachments/assets/06b5d7ad-e6af-4c21-ac05-3b8ea44427fb" width="20">
        AWS RDS<br/>
  <hr style="margin:4px 0;"/>
      <b>📧 Email</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/c3a24ead-982e-443c-affa-8d5b3f10a6f4" width="20">
        JavaMailSender<br/>
      <hr style="margin:4px 0;"/>
      <b>📈 Monitoring</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/592a5e38-ada1-4179-89f3-7827f2008b5d" width="20">
        Spring Actuator<br/>
        <img src="https://github.com/user-attachments/assets/47d7f66a-5d55-44a4-b4b4-e167ba3982fb" width="20">
        Prometheus<br/>
        <img src="https://github.com/user-attachments/assets/15babcd1-8774-4ec3-b996-c774eff08006" width="20">
        Grafana<br/>
        <img src="https://github.com/user-attachments/assets/8f67e9eb-0f56-4717-8606-ad6559fc284a" width="20">
        AWS CloudWatch<br/>
      <hr style="margin:4px 0;"/>
      <b>👥 Collaboration</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/4c2545a7-e860-4633-98c9-0f21de57b642" width="20">
        GitHub<br/>
        <img src="https://github.com/user-attachments/assets/ab197765-c786-4b53-89eb-bb568604f9b1" width="20">
        Slack<br/>
        <img src="https://github.com/user-attachments/assets/37046169-d290-4cb6-b0a2-4cc95f033fb6" width="20">
        Notion<br/>
        <img src="https://github.com/user-attachments/assets/039d9816-7763-48b6-ba1b-443b3a1bfa62" width="20">
        ERD Cloud<br/>
      <hr style="margin:4px 0;"/>
      <b>🏁 Test</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/11e33ee8-b2a0-4862-a870-4a282de700ab" width="20">
        Spring REST Docs<br/>
        <img src="https://github.com/user-attachments/assets/4b8d9320-1cf4-43e7-9536-0d3a067d200c" width="20">
        Postman<br/>
        <img src="https://github.com/user-attachments/assets/33e71bf3-7ff7-4c53-bb0c-70ee2989b8d3" width="20">
        Swagger<br/>
        <img src="https://github.com/user-attachments/assets/938c0518-4b11-4d16-9ab8-5b9fb670b67e" width="20">
        JMeter
        <img src="https://github.com/user-attachments/assets/98e4eb26-b4e8-4214-8824-3db03e9f0af5" width="20">
        K6<br/>
    </td>
    <td valign="top">
      <b>🌐 IDE</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/b47db86a-ccbc-4985-972f-cab71291c4d5" width="20">
        IntelliJ IDEA<br/>
      <hr style="margin:4px 0;"/>
      <b>🔐 Security</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/28847a87-77af-431e-8dd4-0e264b341f22" width="20">
        JWT<br/>
        <img src="https://github.com/user-attachments/assets/90f80753-ddb0-4b0c-a39f-db8733faceda" width="20">
        Spring Security<br/>
      <hr style="margin:4px 0;"/>
      <b>⏰ Batch</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/32d5c547-0e2e-40ff-9f4c-4367f7a35072" width="20">
        Spring Batch<br/>
      <hr style="margin:4px 0;"/>
      <b>✉️ Inter-Service Communication</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/a64f8a72-cbe9-4340-9a32-78db0e3ed970" width="20">
        AWS SQS<br/>
        <img src="https://github.com/user-attachments/assets/6a94ecdd-41a6-4562-8c4e-236757fbd173" width="20">
        gRPC<br/>
      <hr style="margin:4px 0;"/>
      <b>🤖 AI</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/436ff58c-ae62-457a-bda7-fc012310cc20" width="20">
        Google Gemini API<br/>
        <img src="https://github.com/user-attachments/assets/413a0610-9fc1-4ba4-880b-0fac401e9171" width="20">
        Spring AI<br/>
      <hr style="margin:4px 0;"/>
      <b>✈️ CI/CD</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/c3e869c7-eb77-4038-9ae4-4f4e47fb6258" width="20">
        GitHub Actions<br/>
        <img src="https://github.com/user-attachments/assets/9ca85329-fd21-4f17-94cb-15b82444c5d6" width="20">
        Docker<br/>
      <hr style="margin:4px 0;"/>
      <b>☁️ Infra</b><br/><br/>
        <img src="https://github.com/user-attachments/assets/b84694c3-a527-4c79-9160-88eea4ae07c4" width="20">
        Terraform<br/>
        <img src="https://github.com/user-attachments/assets/6fec3049-6d04-4d0e-a600-e1bca88c426d" width="20">
        AWS ECR<br/>
        <img src="https://github.com/user-attachments/assets/ff0128e3-fb55-4e52-92a8-7545266043ae" width="20">
        AWS ECS<br/>
        <img src="https://github.com/user-attachments/assets/a78f6ec1-9820-4870-a92b-c14615c1c2ab" width="20">
        AWS Fargate<br/>
        <img src="https://github.com/user-attachments/assets/42316717-cddd-41e2-8cd0-ca4603c8959c" width="20">
        AWS EC2<br/>
        <img src="https://github.com/user-attachments/assets/7af1a9d3-9059-41b9-bb99-ead69a9b6295" width="20">
        AWS S3<br/>
        <img src="https://github.com/user-attachments/assets/be36b5b8-07b5-4f0b-a066-249cdb080c82" width="20">
        AWS Route53<br/>
        <img src="https://github.com/user-attachments/assets/ee7a5e64-94c2-4307-a821-a250a505453c" width="20">
        AWS CloudFront<br/>
        <img src="https://github.com/user-attachments/assets/e562c5ef-b4df-42ba-9f95-4a1c4f1e6684" width="20">
        AWS Lambda<br/>
        <img src="https://github.com/user-attachments/assets/fbc722fa-f789-496d-ab19-f388133ca702" width="20">
        AWS EventBridge<br/>
        <img src="https://github.com/user-attachments/assets/77b7fa50-7d31-4c89-af3c-538df79691fe" width="20">
        Cloud Map<br/>
        <img src="https://github.com/user-attachments/assets/9079c1a6-0726-4403-90f1-c87c779096d1" width="20">
        AWS ALB<br/>
        <img src="https://github.com/user-attachments/assets/21914b42-f5fc-46d7-8dba-9b50005366ee" width="20">
        AWS NAT Gateway<br/>
        <img src="https://github.com/user-attachments/assets/77434834-d360-4d0d-8fb2-2bb1a179fa0b" width="20">
        Internet Gateway<br/>
        <img src="https://github.com/user-attachments/assets/878c936f-a6b9-46a7-85cc-8f30be6fbc57" width="20">
        AWS IAM<br/>
        <img src="https://github.com/user-attachments/assets/6097a120-c893-4f57-8617-3e4a4defd257" width="20">
        AWS Certificate Manager<br/>
    </td>
  </tr>
</table>
