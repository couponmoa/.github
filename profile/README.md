![배너 이미지](./images/couponmoa_banner.png)
# 🎟️ 구독 기반 실시간 쿠폰 뷰어 서비스
## 1. 팀원 소개
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

## 🔁 서비스 플로우

![서비스플로우 이미지](./images/service_flow.png)

## 🔥 주요 기능
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
