# 5️⃣ 응용 계층 - HTTP의 기초

## DNS

### DNS의 정의
- **DNS(Domain Name System)**: 도메인 이름을 IP 주소로 변환하는 시스템
- **전화번호부 역할**: "www.example.com" → "1.2.3.4" 매핑

### 도메인 이름 구조
#### 계층적 구조
- **점(.)으로 구분된 계층 구조**
- **FQDN**: Fully Qualified Domain Name (전체 도메인 주소)

#### 도메인 구성 예시 (www.example.com)
| 계층 | 설명 |
|------|------|
| **루트 도메인** | . (생략 가능) |
| **최상위 도메인** | com |
| **2차 도메인** | example |
| **3차 도메인** | www |

### DNS 질의 과정
#### 리졸빙(Resolving) 단계
1. 클라이언트의 도메인 IP 요청
2. 로컬 DNS 서버 확인
3. 캐시 미존재 시 계층적 질의:
   - 루트 네임 서버 (.)
   - TLD 네임 서버 (com)
   - 권한 네임 서버 (example.com)
4. IP 주소 획득 및 전송

### DNS 캐시
#### 캐시 필요성
- **여러 서버 거치는 DNS 질의의 지연 방지**
- **반복 요청 최적화**

#### 캐시 위치
- 클라이언트
- 로컬 DNS 서버
- 애플리케이션(브라우저)

#### TTL (Time To Live)
- **캐시 유효 시간**
- **DNS 레코드별 설정**
- **만료 시 재질의**

### DNS 서버 종류
| 서버 종류 | 설명 |
|-----------|------|
| **로컬 네임 서버** | 클라이언트/ISP의 가장 가까운 DNS 서버 |
| **루트 네임 서버** | 전 세계 13개 클러스터, TLD 서버 위치 제공 |
| **TLD 네임 서버** | .com, .net, .kr 등 TLD 관리 |
| **권한 네임 서버** | 실제 도메인-IP 매핑 보유 |

### 공개 DNS 서버
| 제공자 | 주소 |
|--------|------|
| **Google Public DNS** | 8.8.8.8, 8.8.4.4 |
| **Cloudflare** | 1.1.1.1 |

### DNS 레코드
#### 주요 레코드 타입
| 타입 | 설명 |
|------|------|
| **A** | 도메인 ↔ IPv4 주소 |
| **AAAA** | 도메인 ↔ IPv6 주소 |
| **CNAME** | 별칭 ↔ 실제 도메인 |
| **NS** | 도메인 관리 네임서버 |
| **MX** | 메일 서버 설정 |

#### 레코드 예시
| 타입 | 이름 | 값 | TTL |
|------|------|-----|-----|
| **A** | example.com | 1.2.3.4 | 300 |
| **CNAME** | www.example.com | example.com | 300 |

### 도메인 등록 후 작업
#### 필수 작업
- **A 레코드/CNAME 레코드 등록**
- **IP 매핑 정보 설정**

#### DNS 관리 항목
- 레코드 이름
- 레코드 타입
- 값(IP/도메인)
- TTL 설정

## URI/URL

### 자원(Resource)이란?
- **웹 상에서 접근 가능한 모든 대상**
- **예시**: HTML 문서, 이미지, JSON 응답, 텍스트 파일 등
- **식별**: URI(Uniform Resource Identifier)를 통해 식별

### URI의 종류
| 구분 | 설명 |
|------|------|
| **URI** | 웹 상의 자원을 식별하기 위한 표준 식별자 |
| **URN** | 이름(Name) 기반 식별자 (위치와 무관) |
| **URL** | 위치(Location) 기반 식별자 (브라우저 주소창에 입력하는 형식) |

※ URL은 URI의 하위 개념

### URL의 구성 요소
예시: `foo://www.example.com:8042/over/there?name=ferret#nose`

| 요소 | 설명 |
|------|------|
| **scheme** | 접근 방식 (http, https, ftp 등) |
| **authority** | 도메인명 or IP, 포트 포함 |
| **path** | 자원의 위치 경로 |
| **query** | 추가 파라미터 (쿼리 스트링) |
| **fragment** | 자원의 내부 조각 (섹션) |

### 구성 요소 상세 설명

#### scheme
- **자원 접근 방식**: http, https, ftp, mailto, tel, data, file 등
- **예시**: `https://example.com` → HTTPS 프로토콜 사용

#### authority
- **호스트 정보 + 포트** (기본값은 생략 가능)
- **예시**:
  - `www.example.com` → 도메인
  - `192.168.0.1:8080` → IP + 포트

#### path
- **자원이 위치한 경로**
- **슬래시(/)로 구분된 계층적 구조**
- **예시**: `/home/images/a.png`

#### query
- **추가적인 검색 정보**
- **? 뒤에 키=값 쌍으로 구성되고, &로 연결**
- **쿼리 스트링 예시**: `?location=seoul&rooms=2&size=100&min_price=200000`

#### fragment
- **자원의 내부 특정 지점(섹션) 지칭**
- **# 뒤에 위치**
- **예시**: `#section-1.1.2` → HTML 문서의 특정 아이디(#)에 해당하는 부분으로 이동

### URL 예시 분석

#### Query 예시
`http://example.com/search?location=seoul&rooms=2&size=100&min_price=200000`

| 파라미터 | 설명 |
|----------|------|
| location=seoul | 서울 지역 |
| rooms=2 | 침실 2개 |
| size=100 | 100㎡ |
| min_price=200000 | 최소 가격 20만원 |

#### 복합 Query 예시
`http://example.com/search?category=books&brand=hanbit&discounted=true&sorted=price_desc`

| 파라미터 | 설명 |
|----------|------|
| category=books | 카테고리: 책 |
| brand=hanbit | 브랜드: 한빛 |
| discounted=true | 할인 적용 상품 |
| sorted=price_desc | 가격 내림차순 정렬 |

#### Fragment 예시
1. `https://datatracker.ietf.org/doc/html/rfc3986`  
   - 전체 문서
2. `https://datatracker.ietf.org/doc/html/rfc3986#section-1.1.2`
   - `#section-1.1.2` → 문서 내 해당 섹션으로 바로 이동


## HTTP의 특징

### HTTP의 정의
- **HTTP(HyperText Transfer Protocol)**: 웹 자원(HTML, JSON, 이미지 등)을 클라이언트-서버 간에 주고받는 비연결성 기반 전송 프로토콜
- **애플리케이션 계층 프로토콜**: 웹 브라우저와 웹 서버 간의 통신 규약

### HTTP의 주요 특징
#### 1. 요청-응답 기반 프로토콜
- 클라이언트가 요청을 보내고 서버가 응답하는 구조
- 단방향 통신 방식

#### 2. 미디어 독립적
- MIME 타입에 따라 다양한 자원 송수신 가능
- 예: text/html, image/png 등

#### 3. 무상태(Stateless)
- 서버는 클라이언트 상태를 기억하지 않음
- 세션/쿠키 등 별도 관리 필요

#### 4. 지속 연결
- 동일 TCP 연결로 여러 요청 가능
- keep-alive 기술 사용

## HTTP 메시지 구조
### 요청 메시지 (Request Message)
| 구성 요소 | 설명 |
|-----------|------|
| **요청 라인** | 메서드 + 요청 대상 + HTTP 버전 |
| **헤더 필드** | Host, Accept 등 |
| **본문** | (선택적) POST, PUT 등에서 데이터 전송 |

### 응답 메시지 (Response Message)
| 구성 요소 | 설명 |
|-----------|------|
| **상태 라인** | HTTP 버전 + 상태 코드 + 이유 구문 |
| **헤더 필드** | Content-Type, Content-Length 등 |
| **본문** | 요청 결과로 전달되는 데이터 |

### MIME 타입
#### 형식
- 타입/서브타입
- 예: application/json, image/jpeg

#### 주요 타입
| 타입 | 설명 | 예시 (서브타입) |
|------|------|----------------|
| text | 텍스트 | plain, html, css, javascript |
| image | 이미지 | png, jpeg, webp, gif |
| video | 비디오 | mp4, webm, ogg |
| audio | 오디오 | wav, midi |
| application | 바이너리 | json, pdf, xml, x-www-form-urlencoded |
| multipart | 다중 전송 | form-data, encrypted |

### 연결 방식
#### 비지속 연결
- 요청-응답마다 TCP 연결을 새로 맺고 끊음
- HTTP 1.0 방식

#### 지속 연결
- TCP 연결을 재사용하여 여러 요청 처리
- HTTP 1.1 이상
- keep-alive: 지속 연결의 대표 기술

### HTTP 버전 비교

| 항목 | HTTP/1.1 | HTTP/2.0 | HTTP/3.0 |
|------|----------|----------|----------|
| 전송 형식 | 텍스트 기반 메시지 | 바이너리 프레이밍 | 바이너리 + UDP 기반 QUIC |
| 멀티 요청 처리 | ❌ 순차 처리 | ✅ 멀티플렉싱 | ✅ 멀티플렉싱 + 더 빠른 초기 연결 |
| 헤더 압축 | ❌ 없음 | ✅ HPACK 압축 | ✅ QPACK |
| 지속 연결 | ✅ Keep-Alive | ✅ Keep-Alive | ✅ 연결 유지됨 |
| 보안 (TLS) | 선택 사항 | 선택 사항 | 기본 TLS 1.3 내장 |
| 서버 푸시 | ❌ 없음 | ✅ 가능 | 🚫 삭제됨 |
| HOL Blocking | ❌ 매우 심함 | ✅ 개선됨 | ✅ 더 강하게 개선됨 |
| 전송 계층 | TCP | TCP | UDP + QUIC |

### HTTP/2.0 핵심 기술
| 기술명 | 설명 |
|--------|------|
| 멀티플렉싱 | 하나의 연결로 여러 요청/응답을 병렬 전송 |
| 서버 푸시 | 클라이언트 요청 없이 필요한 리소스를 선제 전송 |
| 헤더 압축 | HPACK으로 헤더 크기를 줄여 전송 효율 개선 |
| 바이너리 프레이밍 | 메시지를 바이너리 프레임 단위로 분리해 전송 |

### HTTP/3.0 특징
#### 주요 특징
- TCP → UDP 기반 QUIC 프로토콜 사용
- HOL Blocking 완전 제거
- TLS 1.3 내장
- 빠른 연결 수립
- UDP 기반이지만 신뢰성 보장

#### 적용 현황
- Google, Cloudflare, YouTube 등에서 실서비스 적용 중
- HTTP/3는 "TCP 포기하고, UDP 위에서 HTTP/2 기능을 다 해보자"는 시도

## HTTP 메서드

### 메서드
| 메서드 | 설명 |
|--------|------|
| **GET** | 자원을 조회 |
| **HEAD** | GET과 동일하되 본문 없이 헤더만 응답 |
| **POST** | 서버에 새로운 리소스 생성 또는 처리 요청 |
| **PUT** | 자원을 전체 교체(덮어쓰기) |
| **PATCH** | 자원을 부분 수정 |
| **DELETE** | 자원을 삭제 |
| **CONNECT** | 프록시 터널 연결 시작 |
| **OPTIONS** | 해당 URI에서 가능한 메서드 조회 |
| **TRACE** | 루프백 테스트 (디버깅용) |

### 메서드 상세 예시
#### GET vs HEAD
GET: 자원을 조회하고 응답 본문 포함
HEAD: 본문 없이 응답 헤더만 받음 (리소스 존재 여부 확인 시 사용)
```http
GET /example-page HTTP/1.1
Host: www.example.com
Accept: *

// 응답
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 648
```
```http
HEAD /example-page HTTP/1.1
Host: www.example.com
Accept: *

// 응답
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 648
// (본문 없음)
```

#### POST
서버에 새로운 데이터를 생성하거나 처리 요청
본문에 JSON, Form 등 포함

```http
POST /posting HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "Title": "컴퓨터 네트워크",
  "Contents": "끝까지 화이팅해서 읽어주세요!"
}

// 응답
HTTP/1.1 201 Created
Location: /posting/1
{
  "id": 1,
  "Title": "컴퓨터 네트워크",
  ...
}
```

#### PUT vs PATCH
| 구분 | 설명 | 결과 |
|------|------|------|
| **PUT** | 전체 자원을 덮어씀 | 자원 전체가 교체됨 |
| **PATCH** | 일부 필드만 수정 | 자원 일부만 변경됨 |

#### PUT 예시
```http
PUT /posting HTTP/1.1
{
  "id": 1,
  "Title": "수정된 제목입니다"
}
// 결과: 기존 자원이 완전히 이 JSON으로 덮어쓰기
```

#### PATCH 예시
```http
PATCH /posting HTTP/1.1
{
  "Title": "수정된 제목입니다"
}
// 결과: 기존 자원의 title만 바뀜, 나머지는 그대로
```

#### DELETE
특정 자원을 서버에서 삭제

```http
DELETE /texts/a.txt HTTP/1.1
Host: example.com

// 응답: 200 OK or 204 No Content
```

## 상태 코드

### 상태 코드란?
- 3자리 정수로 구성된 코드
- 요청에 대한 서버의 처리 결과를 알려줌
- 앞자리 숫자(백의 자리)에 따라 의미가 구분됨

### 상태 코드 분류
| 범위 | 이름 | 설명 |
|------|------|------|
| 1xx | 정보 응답 | 요청 수신 중, 처리 진행 중 안내 |
| 2xx | 성공 응답 | 요청 정상 처리 완료 |
| 3xx | 리다이렉션 | 자원 위치 변경 안내 |
| 4xx | 클라이언트 오류 | 요청에 문제 있음 |
| 5xx | 서버 오류 | 서버 내부 처리 실패 |

### 주요 상태 코드

#### 200대: 성공 상태 코드
| 상태 코드 | 이유 구문 | 설명 |
|-----------|-----------|------|
| 200 | OK | 가장 일반적인 성공 응답 |
| 201 | Created | POST 요청 등으로 새 자원이 생성됨 |
| 202 | Accepted | 요청은 수락되었으나 아직 처리 안 됨 (비동기 작업 등) |
| 204 | No Content | 요청 성공, 응답 본문 없음 (예: DELETE 성공 후) |

#### 300대: 리다이렉션 상태 코드
클라이언트에게 자원의 위치가 바뀌었음을 알림

| 상태 코드 | 이유 구문 | 설명 |
|-----------|-----------|------|
| 301 | Moved Permanently | 영구 리다이렉션, URL이 바뀜 (GET/POST → GET 될 수도 있음) |
| 302 | Found | 일시 리다이렉션 (GET으로 바뀔 수 있음) |
| 303 | See Other | GET으로만 재요청하라는 명시적 응답 |
| 307 | Temporary Redirect | 요청 메서드를 유지하며 일시 리다이렉션 |
| 308 | Permanent Redirect | 요청 메서드를 유지하며 영구 리다이렉션 |
| 304 | Not Modified | 캐시된 데이터 사용 가능 → 조건부 요청 응답용 |

#### 리다이렉션 비교
| 상태 코드 | 리다이렉션 유형 | 메서드 변경 가능성 |
|-----------|----------------|-------------------|
| 301 | 영구 | 변경 가능 (GET될 수도) |
| 302 | 일시 | 변경 가능 (GET될 수도) |
| 303 | 일시 | 무조건 GET으로 변경 |
| 307 | 일시 | 변경 안됨 |
| 308 | 영구 | 변경 안됨 |

#### 400대: 클라이언트 에러 상태 코드
| 상태 코드 | 이유 구문 | 설명 |
|-----------|-----------|------|
| 400 | Bad Request | 요청 형식 오류, 유효성 실패 등 |
| 401 | Unauthorized | 인증이 필요함 (Authorization 헤더 필요) |
| 403 | Forbidden | 권한 없음 (인증되어도 거부됨) |
| 404 | Not Found | 해당 URI에 자원이 없음 |
| 405 | Method Not Allowed | 메서드가 지원되지 않음 (예: POST만 허용인데 GET 보냄) |

#### 500대: 서버 에러 상태 코드
| 상태 코드 | 이유 구문 | 설명 |
|-----------|-----------|------|
| 500 | Internal Server Error | 서버 내부 오류, 원인 불명 |
| 502 | Bad Gateway | 중간 게이트웨이/프록시 서버에서 오류 응답 받음 |


## HTTP 주요 헤더

### 1. 요청 메시지 헤더
| 헤더 | 설명 | 예시 |
|------|------|------|
| Host | 요청을 보낸 대상의 도메인 (HTTP/1.1 필수) | Host: info.cern.ch |
| User-Agent | 요청을 보낸 클라이언트(브라우저/앱 등)의 정보 | User-Agent: Mozilla/5.0 ... Firefox/109.0 |
| Referer | 현재 요청 직전 페이지의 주소 (이전 페이지 URL) | Referer: https://minchul.net |

> User-Agent와 Referer는 마케팅, 트래픽 분석, 보안 쪽에서도 많이 활용됨.

### 2. 응답 메시지 헤더
| 헤더 | 설명 | 예시 |
|------|------|------|
| Server | 응답한 서버의 소프트웨어 정보 | Server: Apache/2.4.1 (Unix) |
| Allow | 해당 URI에서 사용 가능한 HTTP 메서드 명시 | Allow: GET, POST |
| Location | 리다이렉션 또는 새 자원의 URI 지정 | Location: /new-path |

### 3. 공통 헤더
| 헤더 | 설명 | 예시 |
|------|------|------|
| Date | 메시지 생성 시간 | Date: Tue, 15 Nov 1994 08:12:31 GMT |
| Content-Length | 메시지 본문 크기 (Byte) | Content-Length: 123 |
| Content-Type | MIME 타입 (본문 포맷 명시) | Content-Type: text/html; charset=UTF-8 |
| Content-Language | 본문에 사용된 언어 | Content-Language: ko-KR |
| Content-Encoding | 본문 압축/변환 방식 | Content-Encoding: gzip |
| Connection | 연결 유지 여부 지정 | Connection: keep-alive / close |

### Content-* 계열 헤더 상세

#### Content-Type
- MIME 타입 명시
- 예: text/html, application/json

#### Content-Language
- 사용 언어 및 국가 (언어코드-국가코드)
- 예: ko-KR, en-US, zh-CN

| 언어 | 언어 코드 | 국가 | 국가 코드 |
|------|-----------|------|-----------|
| 한국어 | ko | 한국 | KR |
| 영어 | en | 미국 | US |
| 중국어 | zh | 중국 | CN |
| 일본어 | ja | 일본 | JP |

#### Content-Encoding
- 본문 압축 방식 명시
- 예: gzip, br, deflate
- 다중 인코딩 가능: Content-Encoding: deflate, gzip
  
```http
Content-Encoding: deflate, gzip
```

#### Connection 헤더
| 값 | 설명 |
|------|------|
| keep-alive | 지속 연결 유지 (HTTP/1.1 이상 기본) |
| close | 요청-응답 이후 TCP 연결 종료 |

# 6. 응용 계층 - HTTP의 응용

## 쿠키
### 개요
- HTTP는 Stateless 프로토콜로 상태 유지가 불가능
- 사용자 상태 유지가 필요할 때 쿠키 사용 (예: "3일간 보지 않기")

### 구조
- 기본 구조: 이름=값 형식의 데이터
- 서버가 Set-Cookie로 쿠키를 전송하면, 이후 클라이언트는 Cookie 헤더로 자동 포함

### 쿠키 전달 흐름
- 응답 시 서버 → 클라이언트

    ```http
    Set-Cookie: name=minchul
    Set-Cookie: phone=100-100
    요청 시 클라이언트 → 서버
    ```

    ```http
    Cookie: name=minchul; phone=100-100
    ```

### 쿠키 속성들
| 속성 | 설명 | 예시 |
|------|------|------|
| Domain | 적용 가능한 도메인 제한 | Set-Cookie: name=...; Domain=minchul.net |
| Path | 적용 경로 제한 | Path=/lectures |
| Expires | 유효기간 (절대 시간) | Expires=Fri, 23 Aug 2024 09:00:00 GMT |
| Max-Age | 유효기간 (초 단위 상대시간) | Max-Age=2592000 |
| Secure | HTTPS 연결에서만 전송 | Secure |
| HttpOnly | JS 접근 차단 (XSS 대응) | HttpOnly |

### 예시 정리
```http
Set-Cookie: sessionID=abc123; Expires=Fri, 23 Aug 2024 09:00:00 GMT
Set-Cookie: sessionID=abc123; Max-Age=2592000
Set-Cookie: sessionID=abc123; Secure
Set-Cookie: sessionID=abc123; HttpOnly
```
> Secure + HttpOnly 조합은 보안이 중요한 로그인 쿠키에 필수

## 웹 스토리지 (Web Storage)
### 차이점: 쿠키 vs 웹스토리지
| 항목 | 쿠키 | 웹 스토리지 |
|------|------|------|
| 저장 위치 | 브라우저 | 브라우저 |
| 저장 주체 | 서버 또는 JS | JS |
| 서버 전송 여부 | 매 요청마다 자동 포함됨 | ❌ 절대 전송되지 않음 |
| 용도 | 세션 유지, 인증 | 사용자 설정, 캐시 등 클라이언트 전용 |
| 용량 | 수 KB 제한 | 수 MB 이상 저장 가능 |

### Web Storage 종류
| 종류 | 유지 범위 | 특징 |
|------|----------|------|
| LocalStorage | 영구 저장 | 브라우저 닫아도 유지됨 |
| SessionStorage | 탭/세션 단위 | 브라우저 탭 닫히면 사라짐 |

## 캐시

### HTTP 캐시 개념 및 역할
- 서버에서 받은 응답(자원)을 임시 저장해놓고, 같은 요청이 반복될 때 저장된 자원을 재활용하여 서버 요청을 줄이는 기술.

- 목적: 응답 시간 단축, 대역폭 절감, 서버 부하 완화

- 적용 위치:

- 브라우저(Local Cache)

- 중간 서버(Proxy, CDN 등) – 공용 캐시(Public Cache)

### 캐시 저장 방식 및 동작 

- 동일한 요청이 발생하면 서버에 재요청하지 않고 캐시된 사본을 바로 사용

- 이 때 응답에는 다음과 같은 헤더가 포함될 수 있음:

    ```http
    HTTP/1.1 200 OK
    Date: Mon, 05 Feb 2024 12:00:00 GMT
    Content-Type: text/plain
    Content-Length: 100
    Expires: Tue, 06 Feb 2024 12:00:00 GMT
    ```

    또는
    ```http
    Cache-Control: max-age=1200
    ```
    - `Expires`: 명시된 시간 이후 캐시 만료
    - `Cache-Control: max-age`: 응답이 캐시된 시점부터 몇 초간 유효

### 캐시 신선도(Cache Freshness)
- 캐시 데이터가 원본 서버 데이터와 얼마나 동기화되어 있는지를 판단하는 지표

- 캐시 신선도 유지를 위해 `Expires` / `Cache-Control` 등 설정 필요

- 신선도 유지 안 되면 원본과 캐시의 불일치 위험

### 캐시 유효기간 이후 동작 – 조건부 요청
- 유효기간이 지났다고 해서 무조건 다시 서버에서 받아오는 건 아님.
- 조건부 요청을 통해 변경 여부만 확인하고 그대로 재사용할 수도 있음.

#### 1. If-Modified-Since 헤더 사용
```http
GET /index.html HTTP/1.1
Host: www.example.com
If-Modified-Since: Fri, 23 Aug 2024 09:00:00 GMT
```
- ✅ 자원이 변경되었을 경우 → 200 OK + 최신 자원

- ❎ 변경 안 됨 → 304 Not Modified + 메시지 바디 없음 → 캐시 사용 가능

- ❌ 자원이 삭제됨 → 404 Not Found

#### 2. Last-Modified 헤더 사용
- 서버는 Last-Modified 헤더로 자원의 최종 수정일도 알려줄 수 있음.
    
    ```http
    Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
    ```

#### 3. If-None-Match + ETag 사용
- ETag (Entity Tag): 자원의 버전을 식별하는 고유한 해시값

    ```http
    GET /index.html HTTP/1.1
    Host: www.example.com
    If-None-Match: "abc"
    ```

- ✅ 자원이 변경되었음 → 200 OK + 새로운 자원

- ❎ 변경 안 됨 → 304 Not Modified (캐시 계속 사용)

- ❌ 자원이 사라짐 → 404 Not Found



## 콘텐츠 협상

### 정의
- 클라이언트가 "같은 URI 자원"에 대해 선호하는 표현(representation)을 서버에 알려서, 그 중 가장 적절한 표현으로 응답받는 기술

- 표현(representation): 동일 자원의 다양한 응답 형태(예: 언어별 HTML, JSON, 인코딩 방식 등)

### 협상 대상 헤더
| 헤더명 | 역할 |
|------|------|
| Accept | 선호하는 MIME 타입 (ex. `text/html`, `application/json`) |
| Accept-Language | 선호하는 언어 (ex. `ko`, `en-US`) |
| Accept-Encoding | 선호하는 인코딩 방식 (ex. `gzip`, `br`) |

> 이들 헤더는 HTTP 요청 메시지에 포함됨

### 우선순위 지정 방식 (Quality Value, q 값)
- q=1.0 ~ q=0.0 사이 값으로 우선순위 표현 (생략 시 기본값 1.0)

- q=0은 해당 표현을 절대 원하지 않음을 의미

- 값이 클수록 우선순위 높음

#### 예시
```http
Accept-Language: ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7
Accept: text/html, application/xml;q=0.9, text/plain;q=0.6, */*;q=0.5
```
- 한국어(ko-KR, ko) > 영어(en-US, en)

- HTML > XML > 일반 텍스트 > 나머지

### 실무 적용 포인트
- 다국어 서비스: `Accept-Language` 활용해 자동 언어 선택

- API 응답 포맷 분기: `Accept: application/json` vs `Accept: text/html`

- 브라우저/클라이언트 최적화: `Accept-Encoding: gzip` 기반 압축 대응

## 보안: SSL/TLS와 HTTPS
### HTTPS 개요
- HTTPS = HTTP + SSL/TLS → 데이터를 암호화해서 주고받는 프로토콜

- 브라우저 주소창에 자물쇠(🔒) 아이콘이 있으면 SSL/TLS 기반 암호화 통신 사용 중

- SSL은 예전 명칭이고, 현재는 TLS가 표준이며 SSL은 사실상 폐기

### TLS 버전
- TLS도 버전이 있음:

- TLS 1.0 → 1.1 → 1.2 → 1.3 (최신, 성능/보안 향상)

- TLS 1.3이 가장 널리 사용되며, HTTPS에서 사용하는 TLS 동작은 크게 3단계로 나뉨:

### TLS 1.3 메시지 송수신 흐름 (핸드셰이크 과정)
- TCP 소켓 연결 (3-way handshake)

- TLS 핸드셰이크

- 인증서, 키 교환, 암호화 방식 협상 등

- HTTP 메시지 송수신

- 이 때 메시지는 암호화되어 있음

### TLS 1.3 핸드셰이크 순서 (요약)
| 클라이언트 | 서버 |
|------------|------|
| `ClientHello (key_share)` | 
| |`ServerHello`, `key_share`, `EncryptedExtensions`, `Certificate`, `CertificateVerify`, `Finished` |
| `Finished` |  |

- 이후 Application Data 주고받음

### 핸드셰이크 핵심 개념
#### 1. 암호화 통신이 가능하도록 키 교환
- 대칭키를 교환하기 위해 TLS가 사용함

- 이때 사용하는 알고리즘/해시 함수 조합을 → Cipher Suite 라고 함

    ```
    plaintext
    ↓
    TLS_AES_128_GCM_SHA256
    ↓       ↓        ↓
    암호화 알고리즘  해시 알고리즘
    ```

#### 2. 인증
- 클라이언트는 서버의 인증서를 받음 (보통 도메인 기반)

- 인증서 안에는 공개키와 도메인=www.~~~ 등의 정보가 담겨 있음

- 이 인증서가 믿을 수 있는 CA(Certificate Authority) 에 의해 발급되었는지도 확인함

### 인증서 개념
- 인증서 = 공개키 + 도메인 정보 + 발급자 정보 + 유효 기간 등

- 브라우저는 인증서를 열어볼 수 있음 (예: *.hanbit.co.kr)

- 인증서가 진짜인지 검증하기 위해 인증서 서명 확인 → CertificateVerify

### TLS 메시지 정리
| 메시지 | 설명 |
|------|------|
| ClientHello | 사용할 TLS 버전, 지원 암호화 방식 목록, 키 공유 값 등 포함 |
| ServerHello | 위 요청에 대한 응답 + 서버가 선택한 방식 명시 |
| Certificate | 서버 인증서 전달 |
| CertificateVerify | 인증서에 대한 서명 검증 |
| Finished | 모든 핸드셰이크 완료되었음을 의미 |
| Application Data | 이후부터 실제 암호화된 HTTP 데이터 |

### 암호화 통신은 왜 중요한가?
- 중간자 공격(MITM) 방지

- 사용자 정보(로그인, 결제 등) 유출 방지

- 서버-클라이언트 간 통신 내용 보호

- 보안상 이유로 HTTPS는 사실상 필수임 (브라우저도 비HTTPS 차단하거나 경고함)


# 7. 프록시와 안정적인 트래픽

## 포워드 프록시와 리버스 프록시

### 오리진 서버
- 클라이언트가 최종적으로 메시지를 전달받는 서버 (요청을 처리하거나 자원을 생성/반환)   

- 쉽게 말해 ‘진짜 서버’

- 오리진 서버 앞에는 여러 중간 서버가 있을 수 있음 (로드밸런서, 캐시 서버 등)

### 중간 서버
- 클라이언트와 오리진 서버 사이에 위치하여 중계 역할을 수행

- 다양한 이유로 중간 서버가 도입됨:

- 보안

- 성능 향상 (캐싱 등)

- 장애 대응 (다중화)

- 부하 분산 (로드 밸런싱)

### 중간 서버의 종류
#### 1. 프록시 서버 (Forward Proxy)
- 클라이언트 측에 가까이 위치

- 클라이언트가 외부 요청을 직접 보내지 않고 프록시를 통해 전달

- 주로 캐싱, 인터넷 필터링, 익명성 보장 목적

- 동작 예시:

    ```
    클라이언트 → 프록시 → 인터넷 → 오리진 서버
    ```

#### 2. 리버스 프록시 (게이트웨이, Reverse Proxy)
- 오리진 서버 측에 가까이 위치

- 클라이언트는 리버스 프록시가 실제 서버인 것처럼 보임

- 게이트웨이란 표현도 종종 사용됨

주로 다음 기능 수행:

- 로드 밸런싱

- SSL 종료 (TLS 종단)

- 보안 필터링

- 인증, 권한 관리

동작 예시:
```PLAIN TEXT
클라이언트 → 인터넷 → 리버스 프록시 → 여러 오리진 서버 중 하나
```
### 고가용성 개념 보충 (HA, High Availability)
- 가용성 (Availability): 장애 없이 서비스를 사용할 수 있는 시간 비율

- 고가용성 (HA):

- 장애 발생 시에도 중단 없이 동작하는 아키텍처

- 일반적으로 오리진 서버를 다중화 (N개 운영)하여 구현

- 프록시/게이트웨이도 보통 중복 구성

### 요약 도식
#### Forward Proxy 구조
```PLAIN TEXT
클라이언트 → 프록시 → 인터넷 → 오리진 서버
```
#### Reverse Proxy / Gateway 구조
```PLAIN TEXT
클라이언트 → 인터넷 → 게이트웨이(리버스 프록시) → 오리진 서버 1~N
```

## 고가용성: 로드 밸런싱과 스케일링

### 고가용성(High Availability, HA) – 핵심 개념
#### 정의
- 시스템이 장애 상황에서도 지속적으로 서비스 가능한 능력

가용성 = 업타임 / (업타임 + 다운타임)

- 99.999% → “파이브 나인(five nines)”이라 불리며, 연간 5.26분 이하만 다운 가능

| 가용성 수치 | 연간 다운타임 | 주간 다운타임 |
|------------|--------------|--------------|
| 99% | 3.65일 | 1.68시간 |
| 99.9% | 8.77시간 | 10.08분 |
| 99.999% | 5.26분 | 6.05초 |

#### 다운타임의 원인
- 하드웨어 장애, 소프트웨어 버그, 과도한 트래픽, 자연재해 등

#### 장애 허용(Fault Tolerance)과 페일오버(Failover)
- 장애가 아예 발생하지 않도록 설계하는 것보단, 문제 발생 시 빠르게 전환 가능한 구조를 설계하는 게 현실적

#### 상태 감지 방법
- 헬스 체크(Health Check): HTTP, TCP, ICMP 등으로 주기적으로 서버 상태 확인

- 하트비트(Heartbeat): 서버 간 주기적 ping/pong → 응답 없으면 장애로 간주

#### 로드 밸런싱(Load Balancing)
- 과도한 트래픽을 여러 서버에 고르게 분산하는 기술

- **로드 밸런서(Load Balancer)**가 클라이언트의 요청을 여러 서버에 적절히 라우팅

#### 알고리즘 종류
- Round Robin: 순차적으로 분산

- Least Connection: 연결 수 가장 적은 서버에 전달

- 가중치 기반: 성능에 따라 가중치 부여 → 더 강한 서버에 더 많은 요청

- Ex) 서버 성능이 각각 2배 차이면, 4:4:2:2로 분배

- ❗단순히 서버만 많다고 HA가 달성되지 않는다. 분산 비율의 정교함도 필요함.

### 스케일링: 확장성과 탄력성
#### 스케일 업 (Vertical Scaling)
- 기존 서버를 더 좋은 장비로 교체

- 구조 단순하지만 한계 존재 (성능 한계 + 단일 장애점)

#### 스케일 아웃 (Horizontal Scaling)
- 서버 수를 늘리는 방식

- 로드 밸런서와 조합 → 장애 분산 + 유연 확장 가능

#### 오토 스케일링 (Auto Scaling)
- 트래픽 급증/급감 시 자동으로 서버 수 조정

- 주로 클라우드(AWS EC2 ASG, GCP Instance Group 등)에서 지원

#### 보충 포인트
- 티켓팅 시작 직전 → 서버 자동 10대 → 종료 후 3대로 감축

- HA 구성의 핵심은 중복성과 분산이다.

- 중복된 리소스를 가지되, 장애 시 자동 전환 가능해야 함

- 로드 밸런서가 이 전환 구조의 중심이 된다.

- 실제 운영에서는 헬스 체크 주기, 타임아웃, 로드 밸런싱 알고리즘 선정 등이 성능과 장애 대응을 좌우함

- 클라우드 환경에서는 **스케일 아웃 + 오토스케일링** 조합이 사실상 정석


## Nginx로 알아보는 로드 밸런싱

### 1. Nginx란?
- 대표적인 웹 서버 프로그램이며, Reverse Proxy, Forward Proxy, 로드 밸런서, 정적 파일 제공 등의 기능을 제공.

- 설정만 잘하면 컨텐츠 캐싱, 보안 접근 제어, 트래픽 분산까지 구현 가능.

### 2. 기본 환경 가정
- `10.10.10.1`에 Nginx 설치됨.

- 백엔드 서버는 다음과 같이 구성됨:

- `10.10.10.2:80`, `10.10.10.3:80`, `10.10.10.4:80` (로드 밸런싱 대상)

### 3. Nginx 설정 디렉터리 구조 및 핵심 파일
| 경로 | 역할 |
|------|------|
| `/etc/nginx/nginx.conf` | 메인 설정 파일 |
| `/etc/nginx/log/nginx` | access.log / error.log |
| `/etc/nginx/conf.d/*.conf` | 서비스별 설정 파일들 (모듈화 목적) |

```nginx
http {
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  include /etc/nginx/conf.d/*.conf;
}
```

### 4. 실제 로드 밸런싱 설정 예시
```nginx
# 백엔드 서버 그룹 정의
upstream backend {
    server 10.10.10.2:80 weight=1;
    server 10.10.10.3:80 weight=2;
    server 10.10.10.4:80 backup;
}

# 프록시 설정
server {
    listen 80;
    server_name localhost;

    location / {
        proxy_pass http://backend;
    }
}
```
- weight는 서버별 트래픽 비율 조정

- backup은 앞선 서버가 죽었을 때만 사용

- proxy_pass는 요청을 지정한 서버 그룹에 넘김

### 5. 알고리즘 직접 지정 가능
```nginx
upstream backend {
    least_conn;  # 연결 수가 가장 적은 서버로 분산
    server 10.10.10.2:80 weight=1;
    server 10.10.10.3:80 weight=2;
    server 10.10.10.4:80 backup;
}
```

- `least_conn`: 최소 연결 서버 선택

- 그 외 사용 가능 알고리즘: `round-robin`, `ip_hash`, `random`, `hash $request_uri` 등

### 6. 업스트림 vs 다운스트림
| 용어 | 의미 |
|------|------|
| Upstream | 클라이언트 → 오리진 서버 방향 |
| Downstream | 오리진 서버 → 클라이언트 방향 |

> 업/다운은 데이터 흐름 기준, 인바운드/아웃바운드는 네트워크 경계 기준

### 7. 인바운드 / 아웃바운드 트래픽
| 용어 | 의미 |
|------|------|
| Inbound | 외부 → 내부 유입 (ex. 고객 웹 요청) |
| Outbound | 내부 → 외부 송신 (ex. 서버의 외부 API 호출) |
