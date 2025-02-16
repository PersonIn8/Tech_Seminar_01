# 🌿 Spring Security & JWT

[![상세 학습 내용](https://img.shields.io/badge/📘_학습_내용_보러가기-005BBB?style=for-the-badge)](https://github.com/andytjdqls/Security-JWT/blob/main/README.md)
[![발표 자료 보기](https://img.shields.io/badge/📢_발표자료-FF6F00?style=for-the-badge)](발표.PPT)



## 📕 학습 내용

<br>


1. **Spring Security는 인증(Authentication)과 인가(Authorization)을 담당하는 강력한 보안 프레임워크**로, 여러 필터를 통해 요청을 검증함.
2. **기존의 세션 기반 인증**은 서버에서 세션을 관리하는 방식으로 보안성이 높지만 **확장성이 떨어지는 단점**이 있음.
3. **JWT 기반 인증**은 클라이언트가 토큰을 직접 관리하며, 서버 부담을 줄일 수 있지만, 토큰 탈취 시 보안 위험이 존재함.
4. **OAuth 2.0은 API 인증 및 외부 서비스 연동을 위해 사용되며, 액세스 토큰과 리프레시 토큰을 활용하여 인증을 관리**함.
5. **보안 강화를 위해 CSRF, XSS 방어, CORS 설정, 비밀번호 해싱(BCrypt) 등을 적용하여 보안성을 높여야 함**.

<br>


## 📌 학습한 주요 내용

✅ 인증 흐름(Authentication Flow)
- Spring Security 필터 체인 구조 및 요청 흐름 분석
- 세션 기반 인증과 JWT 인증 방식 비교
- OAuth 2.0 인증 과정과 토큰 관리 방식

<br>

✅ 보안 취약점(Security Vulnerabilities)
- CSRF(Cross-Site Request Forgery): 사용자 의도와 무관한 요청 수행 위험
- XSS(Cross-Site Scripting): 악성 스크립트를 삽입하여 클라이언트 공격
- CORS(Cross-Origin Resource Sharing) 문제: 웹 애플리케이션 간 리소스 공유 이슈
- JWT 탈취 및 변조 위험: 토큰 보안 미흡 시 발생하는 위협

<br>

✅ 보안 강화 및 해결 방법
- CSRF 방어: CSRF 토큰 사용 또는 SameSite 속성 활용
- XSS 방어: Content Security Policy(CSP) 적용 및 입력값 검증
- CORS 설정: 적절한 CORS 정책 적용 및 HTTP 헤더 설정
- JWT 보안 강화: 짧은 유효기간 설정, 서명 방식(HMAC vs RSA) 고려, 리프레시 토큰 적용
- 비밀번호 보호: BCrypt를 이용한 비밀번호 해싱 및 저장 정책

<br>


## ❔ QnA

<br>


### ⁉️ **리프레시 토큰이 필요한 이유**
- 액세스 토큰은 보안을 위해 **짧은 만료 시간**을 갖도록 설정됨 (예: 15분 ~ 1시간).
- 사용자가 계속해서 서비스를 이용하려면, **새로운 액세스 토큰을 발급받아야 함**.
- 하지만 **매번 로그인하면 불편하기 때문에**, 리프레시 토큰을 이용하여 **자동으로 새로운 액세스 토큰을 발급**.
- **리프레시 토큰의 보안 위험**:
  - 탈취되면 장기간 사용 가능하기 때문에 **보안이 강화된 저장 방식(httpOnly 쿠키, Secure Storage)**이 필요함.
  - 리프레시 토큰이 유출되었을 경우, **블랙리스트에 추가하여 즉시 폐기 가능**.

### ⁉️ **JWT vs 불투명 토큰: 언제 사용할까?**
- **JWT를 사용하는 경우:**
  - 마이크로서비스 환경(MSA)에서 중앙 인증 서버 없이 자체적으로 토큰 검증이 필요할 때.
  - API 게이트웨이가 토큰 검증을 수행하고 개별 서비스에서 다시 검증할 필요가 없을 때.

- **불투명 토큰을 사용하는 경우:**
  - OAuth 2.0을 통해 API 인증을 수행할 때 (예: Google, Facebook API).
  - 보안성이 중요한 환경에서 토큰을 서버에서 관리하고 검증해야 할 때.
  - 액세스 토큰을 즉시 폐기해야 하는 경우 (JWT는 서버에서 폐기가 어렵지만, 불투명 토큰은 가능).

### ⁉️ **왜 비밀번호 해싱이 필요한가?**
- 비밀번호를 평문(Plain Text)으로 저장하면 데이터베이스가 해킹당할 경우 **모든 사용자 정보가 유출됨**.
- 이를 방지하기 위해 해싱(Hashing)을 사용하여 **비밀번호를 복호화할 수 없도록 변환**.

**BCrypt의 특징**
- **Salt 자동 추가** → 같은 비밀번호라도 **매번 다른 해시 값이 생성됨**.
- **연산 비용 조정 가능** → 보안 수준을 높이기 위해 연산 비용을 설정할 수 있음.
- **Rainbow Table 공격 방어** → Salt 덕분에 Rainbow Table 공격(사전 해시 값 매칭 공격)이 어려움.

### ⁉️ **왜 REST API에서는 CSRF 보호를 비활성화할까?**
- CSRF 공격은 **브라우저의 쿠키 자동 전송**을 악용하는 공격.
- 하지만 REST API에서는 **주로 JWT 기반 인증(Authorization 헤더 사용)**을 사용하기 때문에 **쿠키 기반 인증이 아닌 경우 CSRF 위험이 낮음**.
- 즉, **클라이언트가 직접 Authorization 헤더를 추가해야 하므로, 불필요한 CSRF 보호를 비활성화하는 경우가 많음**.

<br>

## 👨‍👨‍👦‍👦 팀원 소개  
| <img src="https://github.com/wns5120.png" width="200px"> | <img src="https://github.com/JaeHee-devSpace.png" width="200px"> | <img src="https://github.com/andytjdqls.png" width="200px"> | <img src="https://github.com/wild-turkey.png" width="200px"> |
| :---: | :---: | :---: | :---: |
| [유호준](https://github.com/wns5120) | [박재희](https://github.com/JaeHee-devSpace) | [이성빈](https://github.com/andytjdqls) | [김지훈](https://github.com/wild-turkey) |