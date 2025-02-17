# 🌿 Spring Security & JWT

[![상세 학습 내용](https://img.shields.io/badge/📘_학습_내용_보러가기-005BBB?style=for-the-badge)](https://github.com/andytjdqls/Security-JWT/blob/main/README.md)
[![발표 자료 보기](https://img.shields.io/badge/📢_발표자료-FF6F00?style=for-the-badge)](발표자료.pdf)



## 📕 학습 내용

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

## ❔학습 및 예상 QnA

### ⁉️ **리프레시 토큰이 필요한 이유**
- 액세스 토큰은 보안을 위해 **짧은 만료 시간**을 갖도록 설정됨 (예: 15분 ~ 1시간).
- 사용자가 계속해서 서비스를 이용하려면, **새로운 액세스 토큰을 발급받아야 함**.
- 하지만 **매번 로그인하면 불편하기 때문에**, 리프레시 토큰을 이용하여 **자동으로 새로운 액세스 토큰을 발급**.
- **리프레시 토큰의 보안 위험**:
  - 탈취되면 장기간 사용 가능하기 때문에 **보안이 강화된 저장 방식(httpOnly 쿠키, Secure Storage)**이 필요함.
  - 리프레시 토큰이 유출되었을 경우, **블랙리스트에 추가하여 즉시 폐기 가능**.

<br>


### ⁉️ **JWT vs 불투명 토큰: 언제 사용할까?**
- **JWT를 사용하는 경우:**
  - 마이크로서비스 환경(MSA)에서 중앙 인증 서버 없이 자체적으로 토큰 검증이 필요할 때.
  - API 게이트웨이가 토큰 검증을 수행하고 개별 서비스에서 다시 검증할 필요가 없을 때.

- **불투명 토큰을 사용하는 경우:**
  - OAuth 2.0을 통해 API 인증을 수행할 때 (예: Google, Facebook API).
  - 보안성이 중요한 환경에서 토큰을 서버에서 관리하고 검증해야 할 때.
  - 액세스 토큰을 즉시 폐기해야 하는 경우 (JWT는 서버에서 폐기가 어렵지만, 불투명 토큰은 가능).

<br>


### ❓**왜 불투명 토큰보다 JWT를 대중적으로 사용할까요?**
JWT Stateless 인증 가능 → 서버 부담 감소
JWT의 토큰 검증 속도 빠름 → DB 조회 필요 없음 (불투명 토큰은 모든 정보를 DB에 저장, 따라서 조회 필요)
확장성이 뛰어남 → 마이크로서비스에서 유리
JWT 내부에 정보 포함 가능 → API 성능 최적화 (불투명 토큰은 내부에 사용할 수 있는 정보 존재 X)
JWT의 OAuth 2.0과의 높은 호환성


### ⁉️ **왜 비밀번호 해싱이 필요한가?**
- 비밀번호를 평문(Plain Text)으로 저장하면 데이터베이스가 해킹당할 경우 **모든 사용자 정보가 유출됨**.
- 이를 방지하기 위해 해싱(Hashing)을 사용하여 **비밀번호를 복호화할 수 없도록 변환**.

**BCrypt의 특징**
- **Salt 자동 추가** → 같은 비밀번호라도 **매번 다른 해시 값이 생성됨**.
- **연산 비용 조정 가능** → 보안 수준을 높이기 위해 연산 비용을 설정할 수 있음.
- **Rainbow Table 공격 방어** → Salt 덕분에 Rainbow Table 공격(사전 해시 값 매칭 공격)이 어려움.

<br>


### ⁉️ **왜 REST API에서는 CSRF 보호를 비활성화할까?**
- CSRF 공격은 **브라우저의 쿠키 자동 전송**을 악용하는 공격.
- 하지만 REST API에서는 **주로 JWT 기반 인증(Authorization 헤더 사용)** 을 사용하기 때문에 **쿠키 기반 인증이 아닌 경우 CSRF 위험이 낮음**.
- 즉, **클라이언트가 직접 Authorization 헤더를 추가해야 하므로, 불필요한 CSRF 보호를 비활성화하는 경우가 많음**.

<br>


---

### ❓**쿠키의 속도는 왜 상대적으로 빠른 것일까? / 세션의 속도 저하는 왜 일어날까?**
- **서버에 저장**되어 있기 때문에 **사용자의 정보를 조회하고 식별**해야하기 때문에 쿠키나 jwt 보다 상대적으로 **속도가 저하**될 수 밖에 없음

<br>


### ❓**쿠키와 세션을 비교하는 사진에서 왜 2개의 요청이 존재할까?**
- 로그인 요청 / 로그인 성공 후 리다이렉트된 페이지에 접근하는 요청 이렇게 2가지의 요청이 존재하게 됨
- 302 Found 응답 코드를 보면, 로그인 요청에 대해서 로그인 성공 후 리다이렉트가 발생

<br>


### ❓**XSS란?**
- **XSS(교차 사이트 스크립팅)** 은 웹 애플리케이션에서 사용자 입력을 적절히 필터링하지 않을 경우, 공격자가 악성 스크립트(JavaScript 등)를 삽입하여 실행할 수 있는 보안 취약점

<br>

### ❓**CSRF란?**
**CSRF(사이트 간 요청 위조)** 는 공격자가 사용자의 인증된 세션을 악용하여 원하지 않는 요청을 서버에 보내도록 유도하는 공격 기법

<br>

### ❓**CORS란?**
**CORS(교차 출처 리소스 공유)** 는 다른 도메인 간의 요청을 제한하는 브라우저 보안 정책이야. 기본적으로 브라우저는 보안상 이유로 다른 출처(origin)에서 온 요청을 차단

<br>

### ❓**왜 JWT와 Spring Security Framework가 같이 쓰이나요?**
🌠 Spring Security가 제공하는 기능
- Role 기반 접근 제어 (RBAC) - JWT의 role 정보를 확인해서 권한(Authorization) 관리
- 매 요청마다 JWT의 유효성을 검사할 수 있는 JWT 검증 필터 존재
- SecurityContextHolder에 인증 정보를 저장 -> 인증 정보 재사용 가능
- BCryptPasswordEncoder -> 암호화

📍 이러한 기능들을 제공하고 있기에 토큰기반의 Stateless한 인증 시스템을 구축할 수 있기 때문에 사용
<br>

### ❓**세션의 저장소가 서버라 해서 왜 쿠키 크기에 제한이 없어진다고 이야기 할 수 있나요?**
클라이언트의 쿠키 크기 제한을 받지 않음.
클라이언트는 **세션 ID(보통 32~128바이트 크기)**만 저장하고,
실제 데이터는 서버(Redis, DB, 메모리 등)에 저장함.
즉, 클라이언트의 쿠키 크기 제한(4KB) 문제를 해결할 수 있음.
로그인 정보, 사용자 권한, 임시 데이터 등을 더 많이 저장 가능.


<br>

### ❓**쿠키의 크키 제한이 평균적으로 4KB 인가요? ?**
브라우저마다 다르지만, 쿠키 1개의 크기는 보통 4KB(4096바이트)로 제한
이는 초기 웹 환경에서 클라이언트의 저장 공간과 네트워크 비용을 고려하여 4KB로 설계됨.

<br>

### ❓**왜 JWT와 Spring Security Framework를 알기 위해 쿠키, 세션, 토큰에 대해서 알아야하나요?**
웹 인증 방식의 기본 개념이기 때문에 알아야 하며, JWT는 기존 세션 기반 인증과 다른 방식으로 동작하기 때문에 알아야함

<br>

### ❓**PayLoad에 있는 정보를 암호화하는 방식 중 JWE는 무엇인가?**
**JWE (JSON Web Encryption)** 는 JWT의 Payload를 암호화하는 표준 방식이야.
기본적으로 JWT는 Base64Url 인코딩만 되어 있을 뿐, 암호화가 적용되지 않아서 Payload에 포함된 정보가 노출될 위험이 있음.
JWE는 이를 해결하기 위해 Payload를 암호화하여 보안성을 강화



## 👪 발표 QnA

### 사용자가 API 요청시 jwt토큰을 인증할텐데 개발 시에 모든 API에 인증 &인가 로직을 구현해야 하나요. jwt를 사용할때 민감한 정보는 넣지 않는다고 했는데 사용자 식별은 어떻게 하나요?

[답변] : <br>
모든 API에서 인증/인가가 필요한 것은 아닙니다. 로그인 페이지나 공용 데이터 제공 API처럼 인증이 필요 없는 API는 별도의 인증 없이 접근할 수 있습니다. 하지만 사용자 정보 조회, 게시글 작성 등 보호가 필요한 API에서는 JWT를 이용해 인증/인가를 수행해야 합니다.

JWT의 페이로드(payload) 부분에는 여러 클레임(claims) 이 포함되는데, 그중 sub(subject) 클레임에 사용자 ID(고유 식별자)가 포함됩니다. 이를 활용해 서버에서 추가 정보를 조회하여 사용자가 누구인지 식별할 수 있습니다.

### 엑세스 토큰에는 주로 어떠한 정보를 넣나요?

[답변] : <br>

사용자의 인증 정보와 권한 정보
사용자 식별자
권한/역할 정보
만료/발급 시간
토큰 발급자 정보
클라이언트 정보

### 쿠키와 세션은 보통 어느 상황에서 사용하는지 궁금합니다!

[답변] : <br>

쿠키는 클라이언트에서 관리하며, 간단한 데이터 저장과 장기적인 상태 유지(데이터 저장)에 적합합니다.
세션은 서버에서 관리하며, 보안이 중요한 사용자 인증 및 상태 유지에 적합합니다.

✔️ 쿠키
1. 광고 및 트래킹
사용자의 방문 이력을 저장하여 추천 광고를 제공하는 데 사용됨
예: Google Analytics, Facebook Pixel 등.

2. 사용자 환경 설정 저장
다크 모드 여부, 언어 설정 등 사용자 환경을 유지하기 위해 쿠키를 사용
예: "theme=dark; expires=Fri, 01 Jan 2025 12:00:00 UTC;"

---

✔️ 세션
로그인 상태 유지
사용자가 로그인하면 서버에서 사용자 정보를 세션에 저장하고, 클라이언트는 JSESSIONID(또는 토큰)를 쿠키에 저장하여 유지함
이후 요청 시 서버가 해당 세션 정보를 조회하여 인증을 수행

장바구니 기능 (쇼핑몰)
사용자가 로그인하지 않은 상태에서도 장바구니 목록을 유지해야 할 때
세션을 사용하여 사용자가 담은 상품을 저장하고, 구매 페이지로 이동해도 상태가 유지됨



### 분산 서버를 사용한다고 하면, 세션은 어떻게 인증이 가능한가요?

[답변] : <br>

Redis와 같은 인메모리 데이터 저장소를 사용하여 세션 정보를 중앙에서 관리할 수 있도록 함,
모든 서버가 이 저장소에 접근할 수 있어 세션 정보 공유 가능.

### access token은 유효기간이 있어서 refresh token으로 갱신을 해야하는데 refresh토큰은 어떤식으로 보관을 하고 보안처리를 하나요

[답변] : <br>

서버 사이드 보관: 서버에서 보관 -> 데이터베이스나 세션에 암호화하여 저장
HTTP-only 쿠키 사용: 클라이언트 측에서 저장할 경우, HTTP-only 쿠키에 저장하여 자바스크립트로 접근할 수 없게 하고, Secure 속성을 설정해 HTTPS 환경에서만 전송되도록 해야 함
토큰 회전 및 만료 처리: refresh token을 주기적으로 갱신하고, 매번 새로운 토큰을 발급하여 이전 토큰을 무효화하는 방식으로 보안을 강화


## 👨‍👨‍👦‍👦 팀원 소개  
| <img src="https://github.com/wns5120.png" width="200px"> | <img src="https://github.com/JaeHee-devSpace.png" width="200px"> | <img src="https://github.com/andytjdqls.png" width="200px"> | <img src="https://github.com/wild-turkey.png" width="200px"> |
| :---: | :---: | :---: | :---: |
| [유호준](https://github.com/wns5120) | [박재희](https://github.com/JaeHee-devSpace) | [이성빈](https://github.com/andytjdqls) | [김지훈](https://github.com/wild-turkey) |
