# 5. 응용계층 - HTTP의 기초

## DNS와 URI/URL

---

## 도메인 네임과 DNS

### 도메인 네임 (Domain Name)
- 사람이 기억하기 쉬운 형태로 IP 주소를 표현한 것.
- 예: `www.google.com`

### 네임 서버 (Name Server)
- 도메인 이름을 IP 주소로 변환하는 서버.

### DNS 서버 (DNS Server)
- DNS 질의를 받아서 해당 도메인에 대한 IP 주소를 응답하는 서버.
- 여러 종류가 존재: 루트 네임 서버, TLD 네임 서버, 권한 있는 네임 서버 등

---

## 리졸빙(Resolving)
- 사용자가 도메인을 입력하면 DNS를 통해 IP 주소로 변환하는 과정
1. 브라우저/운영체제가 먼저 로컬 캐시 확인
2. 없다면 로컬 네임 서버로 질의
3. 루트 네임 서버 → TLD 네임 서버 → 권한 있는 네임 서버 순으로 조회
4. 결과를 로컬 캐시에 저장하여 재사용

---

## 도메인 네임 구조

예시: `www.example.co.kr`

| 구성 요소        | 설명                          |
|------------------|-------------------------------|
| 루트 도메인      | `.` (생략됨)                   |
| 최상위 도메인(TLD) | `kr`                          |
| 2단계 도메인     | `co`                           |
| 서브 도메인      | `example`                      |
| 호스트 이름      | `www`                          |
| 전체 도메인 네임 | `www.example.co.kr`           |

---

## 도메인 네임 시스템 (DNS)
- 계층 구조의 네임 시스템으로 전 세계적으로 분산되어 운영됨

### 로컬 네임 서버
- 사용자의 ISP나 회사 내부에 위치
- 반복적으로 자주 사용하는 도메인의 캐시를 저장

### 공개 DNS 서버
- Google Public DNS: `8.8.8.8`
- Cloudflare DNS: `1.1.1.1`

### 루트 네임 서버 (Root Name Server)
- 전 세계에 13개의 루트 네임 서버가 운영됨
- TLD 서버 위치 정보 보유

### TLD 네임 서버
- `.com`, `.org`, `.kr` 등 최상위 도메인에 대한 정보를 보유

---

## DNS 캐시
- 조회한 도메인 정보를 일정 시간 동안 저장하여 성능 향상

### TTL(Time to Live)
- DNS 레코드의 유효 시간 (초 단위)
- TTL이 지나면 다시 DNS 질의

---

## DNS 자원 레코드 (Resource Record)
- DNS가 저장하는 실제 데이터
- 도메인과 관련된 정보를 기록

### 주요 DNS 레코드 타입
| 타입 | 설명                           |
|------|--------------------------------|
| A    | 도메인 → IPv4 주소 매핑        |
| AAAA | 도메인 → IPv6 주소 매핑        |
| CNAME| 도메인 → 별칭 지정              |
| MX   | 메일 서버 지정                 |
| NS   | 네임 서버 지정                 |
| TXT  | 부가 정보 (SPF, 도메인 인증 등) |

---

## 자원과 URI/URL

### 자원(Resource)
- 웹에서 접근 가능한 대상(문서, 이미지, 영상 등)

### URI (Uniform Resource Identifier)
- 자원의 위치 또는 이름을 식별하는 문자열
- URI = URL 또는 URN

### URL (Uniform Resource Locator)
- 자원의 위치(주소)를 지정
- 예: `https://www.example.com/index.html`

### URN (Uniform Resource Name)
- 자원의 이름만을 식별, 위치와 무관
- 예: `urn:isbn:0451450523`

---

## URI 구성 요소

예시 URI:  
`https://www.example.com:8080/search?q=dns#section2`

| 구성 요소   | 설명                                            | 예시 내 해당 부분         |
|------------|--------------------------------------------------|----------------------------|
| scheme     | 자원에 접근하기 위한 프로토콜                   | `https`                   |
| authority  | 호스트(도메인) 및 포트 정보                    | `www.example.com:8080`    |
| path       | 자원이 위치한 경로                              | `/search`                 |
| query      | 요청 파라미터 정보                              | `?q=dns`                  |
| fragment   | 문서 내 특정 위치(앵커)                         | `#section2`               |

---

## HTTP의 특징과 메시지 구조

---

## HTTP의 특징

### 1. 요청-응답 기반 프로토콜

- 클라이언트가 서버에게 **요청(request)** 을 보내고, 서버는 해당 요청에 **응답(response)** 을 보내는 방식.

- 기본적으로 **비동기적 요청 처리** 가능.

- 예: 사용자가 브라우저에서 URL 입력 → 서버에 요청 전송 → 응답으로 HTML, 이미지 등 수신.

---

### 2. 미디어 독립적 프로토콜

- HTTP는 전송하는 **데이터 형식(media type)** 에 독립적이다.

- 즉, 텍스트, 이미지, 영상 등 어떤 콘텐츠도 HTTP를 통해 전송 가능.

#### 미디어 타입 (Media Type)

- 콘텐츠의 형식을 명시하기 위해 사용하는 표준 표현 방식.

- 구조: `타입/서브타입`

- 예시:

- `text/html` (HTML 문서)

- `image/png` (PNG 이미지)

- `application/json` (JSON 데이터)

#### 미디어 타입의 추가 표기 방법

- **파라미터**를 통해 상세 정보 전달 가능

- 예시:

- `text/html; charset=UTF-8` → 문자 인코딩을 명시

---

### 3. 스테이트리스 프로토콜 (Stateless Protocol)

- HTTP는 기본적으로 클라이언트의 **상태를 저장하지 않음**.

- 각각의 요청은 **독립적**이며, 이전 요청의 상태를 알 수 없음.

#### 이로 인한 장점 (HTTP 서버 설계 목표)

- **확장성(Scalability)**: 상태 정보 저장 없이 요청 처리 가능 → 서버 확장 쉬움

- **견고성(Robustness)**: 중간에 장애가 생겨도 특정 요청 처리 실패에 영향을 최소화

※ 상태 유지가 필요한 경우 쿠키(Cookie), 세션(Session), 토큰(Token) 등을 사용.

---

### 4. 지속 연결 프로토콜 (Persistent Connection)

#### Keep-Alive 기술

- 기본적으로 요청/응답 후 TCP 연결을 끊지만, **지속 연결(Persistent Connection)** 을 통해 **하나의 연결로 여러 요청 처리 가능**.

- HTTP/1.1부터 기본적으로 Keep-Alive 사용

- 장점:

- **연결 재사용 → 지연 감소**

- TCP 3-way/4-way handshake 오버헤드 줄임

---

### 5. HTTP 버전별 특징
| 버전           | 주요 특징                                                                                                                                                                                                                                |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **HTTP 1.1** | Keep-Alive 기본 지원<br>헤더 중복 문제, HOL Blocking 존재                                                                                                                                                                                        |
| **HTTP 2.0** | - **바이너리 프레이밍(Binary Framing)**: 메시지를 바이너리 형식으로 전송<br><br>- **헤더 압축(Header Compression)**: HPACK 알고리즘으로 헤더 크기 줄임<br><br>- **멀티플렉싱(Multiplexing)**: 하나의 TCP 연결로 여러 메시지 동시 처리<br><br>- **서버 푸시(Server Push)**: 클라이언트 요청 없이도 리소스를 미리 전송 |
| **HTTP 3.0** | - **QUIC 프로토콜 기반** (UDP 사용)<br><br>- 빠른 연결 수립, 지연 최소화<br><br>- 멀티플렉싱 및 TLS 통합 처리 \|<br>                                                                                                                                              |

---

## HTTP 메시지 구조

### 전체 구조

```

<시작 라인>

<헤더 필드>

<빈 줄>

<메시지 본문>

```

---

### 시작 라인 (Start Line)

#### 1. 요청 라인 (Request Line)

- 클라이언트가 서버에 보낸 요청의 첫 줄

- 구조: `메서드 경로 프로토콜/버전`

- 예시: `GET /index.html HTTP/1.1`

| 구성요소 | 설명 |
|----------|------|
| **메서드** | 요청의 종류. 예: GET, POST, PUT, DELETE 등 |
| **경로(Path)** | 요청 대상 자원의 경로 |
| **버전** | 사용 중인 HTTP 버전. 예: HTTP/1.1 |

#### 2. 상태 라인 (Status Line)

- 서버가 응답 시 포함하는 첫 줄

- 구조: `HTTP/버전 상태코드 이유구문`

- 예시: `HTTP/1.1 200 OK`

| 구성요소 | 설명 |
|----------|------|
| **HTTP/버전** | 응답에 사용된 HTTP 버전 |
| **상태 코드** | 응답의 결과 상태 숫자. 예: 200, 404, 500 등 |
| **이유 구문** | 상태 코드에 대한 간단한 설명 (예: OK, Not Found) |

---

### 필드 라인 (Field Line)

#### HTTP 헤더 (HTTP Header)

- 요청이나 응답에 대한 부가 정보를 제공

#### 구성

- 구조: `헤더이름: 헤더값`

- 예시:

```

Content-Type: application/json

Content-Length: 128

Connection: keep-alive

```

#### 헤더 종류

| 분류 | 예시 | 설명 |
|------|------|------|
| **일반 헤더** | Connection, Date | 요청/응답 공통 정보 |
| **요청 헤더** | Host, User-Agent | 요청에 관한 정보 |
| **응답 헤더** | Server, Set-Cookie | 서버 응답 정보 |
| **엔티티 헤더** | Content-Type, Content-Length | 메시지 본문 관련 정보 |

---

### 메시지 전달 흐름 예시

#### 요청 예시 (클라이언트 → 서버)

```

GET /hello.html HTTP/1.1

Host: [www.example.com](http://www.example.com)

Connection: keep-alive

User-Agent: Mozilla/5.0

<본문 없음>

```

#### 응답 예시 (서버 → 클라이언트)

```

HTTP/1.1 200 OK

Content-Type: text/html

Content-Length: 1024

Connection: keep-alive

<html>...</html>

```

## HTTP 메서드와 상태 코드

---

## HTTP 메서드

| 메서드 | 설명 |
|--------|------|
| GET | 서버에서 자원을 조회함. 요청 본문 없음. |
| POST | 서버에 자원을 생성하거나 데이터 전송. 요청 본문 포함. |
| PUT | 자원을 전체 수정. 해당 자원이 없으면 생성. |
| PATCH | 자원을 부분 수정. |
| DELETE | 자원을 삭제. |
| HEAD | GET과 동일하지만, 응답 본문 없이 헤더만 수신. |
| OPTIONS| 서버가 지원하는 메서드 확인. |
| TRACE | 요청을 그대로 반환(디버깅용). |
| CONNECT| 터널링용 메서드 (주로 HTTPS). |

---

### 메서드별 요청/응답 예시

#### 1. GET 요청

**요청**

```

GET /users/123 HTTP/1.1

Host: example.com

```

**응답**

```

HTTP/1.1 200 OK

Content-Type: application/json

{

"id": 123,

"name": "Alice"

}

```

---

#### 2. POST 요청

**요청**

```

POST /users HTTP/1.1

Host: example.com

Content-Type: application/json

{

"name": "Bob"

}

```

**응답**

```

HTTP/1.1 201 Created

Location: /users/124

```

---

#### 3. PUT 요청

**요청**

```

PUT /users/123 HTTP/1.1

Host: example.com

Content-Type: application/json

{

"name": "Alice Updated"

}

```

**응답**

```

HTTP/1.1 200 OK

```

---

#### 4. PATCH 요청

**요청**

```

PATCH /users/123 HTTP/1.1

Host: example.com

Content-Type: application/json

{

"name": "Alice Patched"

}

```

**응답**

```

HTTP/1.1 200 OK

```

---

#### 5. DELETE 요청

**요청**

```

DELETE /users/123 HTTP/1.1

Host: example.com

```

**응답**

```

HTTP/1.1 204 No Content

```

---

## HTTP 상태 코드

HTTP 상태 코드는 응답의 결과 상태를 숫자 코드로 표현하며, 다음과 같이 5가지 범주로 나뉩니다:

---

### 1xx (정보 응답)

| 코드 | 이유 구문 | 설명 |
|------|----------------|----------------------------------|
| 100 | Continue | 요청의 일부를 받았으며, 나머지를 보내도 됨 |
| 101 | Switching Protocols | 프로토콜 전환 요청 수락됨 |

---

### 2xx (성공)

| 코드 | 이유 구문 | 설명 |
|------|----------------|--------------------------------|
| 200 | OK | 요청 성공 |
| 201 | Created | 자원 생성 완료 |
| 204 | No Content | 본문 없이 성공 |

---

### 3xx (리다이렉션)

| 코드 | 이유 구문 | 설명 |
|------|------------------|--------------------------------|
| 301 | Moved Permanently| 자원이 **영구적으로** 이동함 |
| 302 | Found | 자원이 **일시적으로** 이동함 |
| 304 | Not Modified | 캐시 사용 가능, 변경 없음 |

#### 리다이렉션(Redirection)

- 클라이언트가 요청한 리소스가 다른 URI로 **이동**했음을 의미.
- 브라우저는 서버가 지정한 새 URI로 자동 요청을 보냄.

#### 리다이렉션 종류

| 유형 | 설명 |
|----------|------|
| **영구적 리다이렉션 (301)** | 앞으로는 새로운 URI를 사용해야 함 |
| **일시적 리다이렉션 (302, 307)** | 임시 이동, 추후 다시 원래 URI 사용 가능 |

---

### 4xx (클라이언트 오류)

| 코드 | 이유 구문 | 설명 |
|------|--------------------|----------------------------------|
| 400 | Bad Request | 문법 오류 등의 잘못된 요청 |
| 401 | Unauthorized | **인증(Authentication)** 실패 |
| 403 | Forbidden | **권한(Authorization)** 없음 |
| 404 | Not Found | 존재하지 않는 자원 요청 |
| 405 | Method Not Allowed | 허용되지 않은 메서드 사용 |

#### 인증 vs 권한

| 항목 | 설명 |
|----------|------|
| 인증(Authentication) | 사용자가 누구인지 확인 (로그인) |
| 권한(Authorization) | 인증된 사용자가 **무엇을 할 수 있는지** 확인 |

---

### 5xx (서버 오류)

| 코드 | 이유 구문 | 설명 |
|------|------------------|----------------------------------|
| 500 | Internal Server Error | 서버 내부 오류 |
| 501 | Not Implemented | 서버가 해당 기능을 지원하지 않음 |
| 502 | Bad Gateway | 게이트웨이/프록시 오류 |
| 503 | Service Unavailable | 서비스 이용 불가 (과부하 등) |

---

## HTTP 주요 헤더

---

## 요청 메시지(Request)에서 주로 활용되는 HTTP 헤더

| 헤더 이름 | 설명 |
|---------------|------|
| **Host** | 요청 대상 서버의 도메인 및 포트 번호를 지정함. <br>예: 가상 호스팅 환경에서 어떤 도메인에 대한 요청인지 서버가 구분할 수 있게 함. <br>**예시:**<br>`Host: www.example.com:8080` |
| **User-Agent** | 요청을 보낸 클라이언트(브라우저, 앱, 봇 등)의 정보를 전달. <br>서버는 이를 통해 사용자 환경에 맞는 응답을 제공할 수 있음. <br>**예시:**<br>`User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)` |
| **Referer** | 현재 요청이 **어떤 페이지에서 유입되었는지**를 나타냄. <br>웹 분석, 보안(CSRF 차단) 등에서 활용. <br>**예시:**<br>`Referer: https://www.google.com/` |

---

## 응답 메시지(Response)에서 주로 활용되는 HTTP 헤더

| 헤더 이름 | 설명 |
|-------------|------|
| **Server** | 응답을 보낸 서버 소프트웨어의 정보를 전달. <br>보안상 노출을 꺼려 설정에서 제거하기도 함. <br>**예시:**<br>`Server: Apache/2.4.41 (Ubuntu)` |
| **Allow** | 해당 자원에 대해 서버가 지원하는 HTTP 메서드를 명시. <br>405 Method Not Allowed 응답 시 함께 전달됨. <br>**예시:**<br>`Allow: GET, POST, HEAD` |
| **Location** | 리다이렉션 시 새롭게 이동할 URL을 제공. <br>301, 302, 201 등의 응답 코드와 함께 사용. <br>**예시:**<br>`Location: https://new.example.com/login` |

---

## 요청과 응답 모두에서 활용되는 공통 HTTP 헤더

| 헤더 이름 | 설명 |
|-------------------|------|
| **Date** | 메시지가 생성된 날짜와 시간 (GMT 기준). <br>**예시:**<br>`Date: Sun, 08 Jun 2025 12:00:00 GMT` |
| **Content-Length** | 본문의 바이트 크기를 명시함. <br>요청/응답 본문 크기를 서버/클라이언트가 예상 가능. <br>**예시:**<br>`Content-Length: 348` |
| **Content-Type** | 본문의 MIME 타입을 지정함. <br>서버는 응답 콘텐츠의 종류를 알려주고, 클라이언트는 본문 포맷을 명시함. <br>**예시:**<br>`Content-Type: application/json` |
| **Content-Language** | 본문의 자연어를 명시. <br>다국어 콘텐츠 제공 시 활용. <br>**예시:**<br>`Content-Language: ko` |
| **Content-Encoding** | 본문 데이터에 적용된 인코딩 방식 지정. <br>압축 방식 등 포함. <br>**예시:**<br>`Content-Encoding: gzip` |
| **Connection** | TCP 연결을 어떻게 처리할지 명시. <br>`keep-alive`는 연결 유지, `close`는 응답 후 연결 종료. <br>**예시:**<br>`Connection: keep-alive` |

---

## 헤더 사용 예시 전체

### 요청 예시

```

GET /index.html HTTP/1.1

Host: [www.example.com](http://www.example.com)

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)

Referer: [https://www.google.com/](https://www.google.com/)

Content-Type: text/html

Connection: keep-alive

```

### 응답 예시

```

HTTP/1.1 200 OK

Date: Sun, 08 Jun 2025 12:00:00 GMT

Server: Apache/2.4.41 (Ubuntu)

Content-Type: text/html; charset=UTF-8

Content-Length: 6821

Content-Language: en

Content-Encoding: gzip

Connection: keep-alive

```

---

# 6. 응용 계층 - HTTP의 응용

---

## 쿠키 (Cookie)

### 쿠키란?

- 클라이언트에 **작은 데이터를 저장**하여 상태를 유지하게 해주는 수단.

- 서버가 `Set-Cookie` 헤더로 쿠키를 설정하면, 이후 클라이언트는 요청마다 해당 쿠키를 `Cookie` 헤더에 실어 보냄.

---

### Set-Cookie 헤더 예시

#### 서버 → 클라이언트 응답 메시지

```

HTTP/1.1 200 OK

Set-Cookie: userId=abc123; Domain=minchul.net; Path=/lectures; Max-Age=3600; Secure; HttpOnly

```

- `userId=abc123`: 쿠키 이름과 값

- `Domain=minchul.net`: 해당 도메인을 포함한 모든 서브도메인에 대해 전송

- `Path=/lectures`: 지정된 경로 이하의 URL에만 쿠키 전송

- `Max-Age=3600`: 쿠키의 유효시간(초) → 1시간 후 삭제됨

- `Secure`: HTTPS 연결에서만 전송

- `HttpOnly`: JavaScript에서 접근 불가 (XSS 방어용)

---

#### 클라이언트 → 서버 요청 메시지

```

GET /lectures/101 HTTP/1.1

Host: [www.minchul.net](http://www.minchul.net)

Cookie: userId=abc123

```

---

## 웹 스토리지(Web Storage)

| 종류 | 설명 |
|-------------|------|
| **LocalStorage** | 브라우저에 **영구 저장**. 브라우저/탭 닫아도 유지됨. |
| **SessionStorage** | **탭 세션 단위 저장**. 탭이나 창을 닫으면 삭제됨. |

- 둘 다 클라이언트 측에서 키-값 쌍을 저장할 수 있는 방식

- 쿠키보다 **용량이 큼 (약 5MB)**, **HTTP 요청 시 자동 전송되지 않음**

---

## 캐시 (Cache)

### 캐시란?

- **자주 요청되는 데이터를 브라우저나 중간 서버에 저장**하여 재사용하는 방식

- 네트워크 지연 감소, 서버 부하 감소

---

### 캐시 제어 헤더

#### Max-Age 예시

```

Cache-Control: max-age=600

```

- 응답을 받은 시점부터 600초(10분) 동안 **신선한 상태**로 유지

#### Expires 예시

```

Expires: Wed, 08 Jun 2025 14:00:00 GMT

```

- 응답이 **언제까지 유효한지** 절대 시간으로 명시

---

### 캐시 신선도(freshness)

- 클라이언트가 **캐시된 응답을 재사용 가능한지 판단**하는 기준

- `max-age`나 `Expires`를 통해 설정

---

### 원본 자원의 변경 여부 확인

#### `If-Modified-Since` 헤더

- 클라이언트가 캐시된 리소스를 보유한 상태에서 **이후 수정된 적이 있는지 질의**

#### 3가지 경우

| 상황 | 서버 응답 |
|------|-----------|
| 자원이 수정됨 | `200 OK` + 새로운 리소스 전송 |
| 자원이 수정 안됨 | `304 Not Modified` (본문 없음, 캐시 사용) |
| 잘못된 시간 포맷 | 무시하고 전체 리소스 재전송 |

---

### ETag (Entity Tag)

- 서버가 리소스의 고유 버전을 식별하기 위해 생성한 **해시값 또는 고유 문자열**

- 캐시 검증 시 사용

#### 예시

```

ETag: "v2.3.6-abcd1234"

If-None-Match: "v2.3.6-abcd1234"

```

- 서버는 해당 ETag가 여전히 유효하다면 `304 Not Modified` 응답

---

## 콘텐츠 협상(Content Negotiation)

### 표현(Representation)이란?

- 같은 자원이라도 **형식(HTML, JSON 등)** 혹은 **언어**에 따라 다양한 표현이 존재

### 주요 협상 헤더

| 헤더 이름 | 설명 | 예시 |
|-----------|------|------|
| **Accept** | 수락 가능한 **MIME 타입**을 명시 | `Accept: application/json, text/html` |
| **Accept-Language** | 수락 가능한 **언어**를 명시 | `Accept-Language: ko-KR, en-US` |
| **Accept-Encoding** | 수락 가능한 **압축 방식** 명시 | `Accept-Encoding: gzip, deflate` |

- 서버는 클라이언트의 요청에 따라 **가장 적절한 표현 방식으로 응답**

---

## 보안 - SSL/TLS와 HTTPS

### SSL/TLS 개념

- **전송 계층 보안 프로토콜**

- 데이터의 기밀성(암호화), 무결성(변조 방지), 인증(위조 방지)을 제공

- **SSL**은 초기 버전 → 현재는 **TLS**가 표준

---

### TLS 통신 과정 (핸드셰이크)

1. 클라이언트 → 서버: 지원 가능한 암호 스위트 목록 전달

2. 서버 → 클라이언트: 인증서 + 암호 스위트 선택

3. 클라이언트: 인증서 확인 및 세션 키 생성

4. 서버와 세션 키 공유 (비대칭 → 대칭 암호 방식 전환)

5. 이후 암호화된 데이터 송수신

---

### 암호 스위트 (Cipher Suite)

- TLS가 사용할 암호화 알고리즘들의 **조합**

- 구성 요소:

- 키 교환 알고리즘

- 대칭키 암호화 알고리즘

- 메시지 인증 코드(MAC) 알고리즘

- 해시 알고리즘

#### 예시

```

TLS\_AES\_128\_GCM\_SHA256

```

- AES-128 암호화 + GCM 인증 + SHA-256 해시

---

### 인증서 (Certificate)

- **서버의 신원을 검증**하는 디지털 문서

- 공개키, 서버 도메인, 인증기관 서명 등 포함

---

### 인증기관(CA: Certificate Authority)

- **신뢰할 수 있는 제3자**로서, 인증서에 서명하여 그 서버의 신뢰성을 보증

- 예: Let's Encrypt, DigiCert, GlobalSign

---

### HTTPS란?

- **HTTP + TLS**

- 기존 HTTP 통신을 **암호화된 보안 채널 위에서 수행**

- 기본 포트: `443`

---

# 7. 프록시와 안정적인 트래픽

---

## 오리진 서버와 중간 서버

### 오리진 서버(Origin Server)

- 사용자가 요청하는 콘텐츠를 **최초로 보유한 서버**

- 예: 웹 페이지, 이미지, API 데이터 등을 직접 저장 및 처리하는 서버

### 프록시(Proxy) 서버

- 클라이언트와 오리진 서버 사이에서 **중계 역할**을 하는 서버

#### 포워드 프록시 (Forward Proxy)

- **클라이언트 측에 위치**하여 내부 사용자가 외부 리소스에 접근할 때 중개

- 예: 내부망 사용자가 특정 IP로 접근 시 중간 프록시를 통해 우회

#### 리버스 프록시 (Reverse Proxy)

- **서버 측에 위치**하여 외부 요청을 받아 실제 서버로 전달

- 보안 강화, 로드 밸런싱, 캐시, SSL 종료 등 다양한 기능 제공

### 게이트웨이(Gateway)

- 서로 다른 네트워크 또는 프로토콜 간의 **중계기 역할**

- 예: API Gateway는 여러 마이크로서비스의 진입 지점 역할

---

## 고가용성: 로드 밸런싱과 스케일링

### 가용성(Availability)

- **서비스가 얼마나 오랫동안 정상적으로 동작하는가**를 나타내는 지표

| 용어 | 설명 |
|-------------|------|
| 업타임(Uptime) | 시스템이 정상 작동하는 시간 |
| 다운타임(Downtime) | 시스템이 중단된 시간 |
| 결함 감내(Fault Tolerance) | 일부 장애가 발생해도 전체 서비스가 유지되는 성질 |

### 페일오버(Failover)

- 장애 발생 시 **자동으로 대체 서버로 전환**하는 기능

- 고가용성(HA, High Availability)을 위한 핵심 메커니즘

### 헬스 체크(Health Check)와 하트비트(Heartbeat)

- **헬스 체크**: 서버나 인스턴스가 정상인지 주기적으로 검사

- **하트비트**: 노드 간에 생존 여부를 판단하기 위한 신호 전송

---

## 로드 밸런싱 (Load Balancing)

### 로드 밸런싱 개념

- 여러 서버에 트래픽을 **효율적으로 분산**하여 병목을 방지하는 기술

- 부하 분산을 통해 성능 향상 및 고가용성 확보

### 로드 밸런서

- 클라이언트 요청을 받아 여러 서버 중 하나에 전달

- 예: Nginx, HAProxy, AWS ELB

### 로드 밸런싱 알고리즘

#### 1. 라운드 로빈(Round Robin)

- 요청을 서버 목록에 따라 **순차적으로 분산**

- 균등한 분산이 가능하나 서버 성능 차이를 고려하지 않음

#### 2. 최소 연결(Minimum Connection)

- 현재 **연결 수가 가장 적은 서버**로 요청을 분배

- 실시간 부하에 따라 유동적으로 대응 가능

#### 기타 알고리즘

| 알고리즘 | 설명 |
|---------------------|------|
| 해시 기반(Hash) | 요청의 해시값을 기반으로 서버 결정 (예: IP 해시) |
| 가중 라운드 로빈 | 서버별 가중치를 설정하여 분산 비율 조정 |
| 응답 시간 기반 | 가장 빠르게 응답한 서버로 분산 |
| 랜덤 선택(Random) | 서버를 무작위로 선택 |

---

## 스케일링(Scaling)

### 수직적 확장(Scale Up)

- 기존 서버의 **CPU, RAM, Disk 등 하드웨어 성능을 증가**

- 간단하지만 물리적 한계가 존재

### 수평적 확장(Scale Out)

- 서버의 **개수를 늘려** 부하를 분산

- 클라우드 환경에서 선호되며 로드 밸런서와 함께 사용

### 오토스케일링(Auto Scaling)

- **트래픽/부하에 따라 서버 수를 자동으로 조절**

- 예: CPU 사용률이 80% 이상이면 서버 인스턴스를 자동 추가

---

## Nginx로 알아보는 로드 밸런싱

### 구성 예시

Nginx 설정에서 다음과 같이 로드 밸런싱을 구성할 수 있다:

```nginx

http {
	upstream backend_servers {
		server backend1.example.com;
		server backend2.example.com;
	}

	server {
		listen 80;
		location / {
			proxy_pass http://backend_servers;
		}
	}
}

````

* `upstream`: 요청을 분산할 **백엔드 서버 그룹**

* `proxy_pass`: 클라이언트 요청을 **백엔드로 전달**

### 인바운드 / 아웃바운드

| 용어 | 설명 |
| --------------- | ----------------- |
| 인바운드(Inbound) | 외부 → 서버로 들어오는 트래픽 |
| 아웃바운드(Outbound) | 서버 → 외부로 나가는 트래픽 |

### 업스트림 / 다운스트림

| 용어 | 설명 |
| ----------------- | ---------------------------------- |
| 업스트림(Upstream) | 클라이언트 기준에서 **데이터 제공자** (예: 오리진 서버) |
| 다운스트림(Downstream) | 클라이언트 기준에서 **데이터 소비자** (예: 사용자) |

Nginx는 클라이언트의 요청을 받아 **업스트림 서버에 분산**하고, 응답을 받아 다시 **다운스트림 클라이언트에 반환**함.
