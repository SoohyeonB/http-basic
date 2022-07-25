# Section 7. HTTP 헤더 - 일반 헤더
## HTTP 헤더 개요
### HTTP 헤더
```
header-filed = field-name ":" OWS fiels-value OWS (OWS: 띄어쓰기 허용)
```
- field-name은 대소문자 구분이 없다.
<br>

### HTTP 헤더 - 용도
- HTTP 전송에 필요한 모든 부가정보 포함
  - Ex) 메시지 바디의 내용, 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등등
- 표준 헤더가 너무 많다.
- 필요 시 임의의 헤더 추가 가능
<br>

### 과거 분류 방법 - RFC2616
- General 헤더: 메시지 전체(요청, 응답 상관없이)에 적용되는 정보
  - Ex) Connection: close
- Request 헤더: 요청을 보낼 때 들어가는 정보
  - Ex) User-Agent: Mozilla/5.0 (Macintosh; ..)
- Response 헤더: 응답에 들어가는 정보
  - Ex) Server: Apache
- Entity 헤더: 엔티티 바디 관련 정보
  - Ex) Context-Type: text/html, Content-Length: 3423
  - 메시지 바디는 엔티티 바디를 전달하는데 사용. 즉, 메시지 바디에 엔티티 바디를 담아 전달
  - 엔티티 바디는 요청이나 응답에서 전달할 실제 데이터
  - 엔티티 헤더는 엔티티 바디의 데이터를 해석할 수 있는 정보를 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등
 <br>
 <br>
 
### HTTP 표준 변화
> RFC2616 폐기  
> 2014년 RFC7230 ~ 7235 등장
- 엔티티 → 표현(Respresentation)
- Representation = Representation Metadata + Representation Data
<br>
<br>

### HTTP 바디 - RFC7230(최신)
- 메시지 바디를 통해 표현 데이터 전달
- 메시지 바디 = 페이로드(payload)
- 표현(= 표현 헤더 + 표현 데이터): 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더: 표현 데이터를 해석할 수 있는 정보 제공
  - Ex) 데이터 유형(html, json), 데이터 길이, 압축 정보 등
<br>
<br>
<br>
<br>

## 표현
```
Content-Type: 표현 데이터의 형식
Content-Encoding: 표현 데이터의 압축 방식
Content-Language: 표현 데이터의 자연 언어
Content-Length: 표현 데이터의 길이
```
- 표현 헤더는 전송, 응답 둘 다에 사용
<br>

### Content-Type
> 표현 데이터의 형식 설명
- 바디에 들어가는 데이터의 형식
- 미디어 타입, 문자 인코딩
- Ex) 
  - text/html; charset-utf-8
  - application/json
  - image/png
<br>

### Content-Encoding
> 표현 데이터 인코딩
- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더를 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보를 이용해 압축 해제
- Ex)
  - gzip
  - deflate
  - identity: 압축 X
<br>

### Content-Language
> 표현 데이터의 자연 언어
- 표현 데이터의 자연 언어를 나타낸다.
- Ex)
  - ko
  - en
  - en-US
<br>

### Content-Length
> 표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding을 사용하면 Content-Length를 사용하면 안된다.
  - Transfer-Encoding 안에 정보가 다 들어있기 때문
<br>
<br>
<br>
<br>

## 콘텐츠 협상
> 클라이언트가 선호하는 표현 요청
```
Accept: 클라이언트가 선호하는 미디어 타입 전달
Accept-Charset: 클라이언트가 선호하는 문자 인코딩
Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
Accept-Language: 클라이언트가 선호하는 자연 언어
```
- 협상 헤더는 요청 시에만 사용
<br>
<br>

### 협상과 우선순위 1
> Quality Values(Q)
- Quality Values(q)값 사용
- 0~1까지, 값이 클수록 높은 우선순위 의미
- 생략 시 1을 의미
<br>

```
GET /event
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
```
#### 우선순위
1. ko-KR;q=1(q 생략)
2. ko;q=0.9
3. en-US;q=0.8
4. en:q=0.7
<br>
<br>

### 협상과 우선순위 2
> Quality Values(q)
- 구체적인 것이 우선순위가 높다.
```
GET /event
Accept: text/*,text/plain,text/plain;format=flowed,*/*
```
#### 우선순위
1. text/plain;format=flowed
2. text/plain
3. text/*
4. */*
<br>

### 협상과 우선순위 3
> Quality Values(q)
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
```
Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5
```
<img width="260" alt="스크린샷 2022-07-21 오후 5 46 26" src="https://user-images.githubusercontent.com/80838501/180171863-92fac62f-780e-4098-9292-2b1c13653b4c.png">
<br>
<br>
<br>
<br>

## 전송 방식
```
단순 전송
압축 전송
분할 전송
범위 전송
```
<br>

### 단순 전송
> Content-Length
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

<html>
  <body>...</body>
</html>
```
- content에 대한 길이를 Content-Legnth로 지정해서 전송 <br>
→ content 길이를 알 수 있을 때 사용
- 한 번에 요청하고 한 번에 받는 것
<br>

### 압축 전송
> Content-Encoding
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Length: 521

asdkjfk;awjhnrefkljabnlkedfjblaksdjfgblkjdsb
```

- 압축해서 전송
- Content-Encoding 헤더를 추가해 어떤 것으로 압축했는지 알려줘야 한다.
<br>

### 분할 전송
> Transfer-Encoding
```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

5
Hello
5
Hello
0
\r\n
```
- 분할헤서 전송하면 오는대로 바로바로 사용 가능
- Content-Length 헤더를 포함하면 안된다.
<br>

### 범위 전송
> Range, Content-Range
```
HTTP/1.1 200 OK
Content-Type: text/plain
Conteent-Range: bytes 1001-2000 / 2000

qefoqwiefhoeiqhfoihoiqehfoihoqiehfoiqhfe
```
- 범위를 지정해 전송
- 중간에 전송하다 끊긴 경우, 처음부터 다시 보내지 않고 범위를 지정해 못받은 부분부터 전송할 때 사용
<br>
<br>
<br>
<br>

## 일반 정보
> 일반 정보를  헤더
```
From: 유저 에이전트의 이메일 정보
Referer: 이전 웹 페이지 주소
User-Agent: 유저 에이전트 애플리케이션 정보
Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
Date: 메시지가 생성된 날짜
```
<br>

### From
> 유저 에이전트의 이메일 정보
- 일반적으로 잘 사용되지 않는다.
- 검색 엔진 같은 곳에서 주로 사용
- 요청에서 사용
<br>

### Referer
> 이전 웹 페이지 주소
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A → B로 페이지 이동하는 경우 B를 요청할 때 `Referer: A`를 포함해서 요청
- Referer를 사용해 유입 경로 분석 가능
- 요청에서 사용
<br>

> 참고
> referer는 referrer의 오타다.
<br>

### User-Agent
> 유저 에이전트 애플리케이션 정
```
user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
```
- 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등)
- 서버 입장에서 통계 정보를 내기 편리하다.
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용
<br>

### Server
> 요청을 처리하는 ORIGIN 서버(마지막 서버)의 소프트웨어 정보
```
Server: Apache/2.2.22 (Debian)
Server: nginx
```
- 응답에서 사용
<br>

### Date
> 메시지가 발생한 날짜와 시간
```
Date: Tue, 15 Nov 1994 08:12:31 GMT
```
- 응답에서 사용
<br>
<br>
<br>
<br>

## 특별한 정보
> 특별한 정보를 제공하는 헤더
```
Host: 요청한 호스트 정보(도메인)
Location: 페이지 리다이렉션
Allow: 허용 간으한 HTTP 메소드
Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
```
<br>

### Host
> 요청한 호스트 정보(도메인)
```
GET /search?q=hello&hl=ko HTTP/1.1
Host: wwww.google.com
```
- 요청에서 사용하는 필수값
- 하나의 서버가 여러 도메인을 처리해야 할 때 
- 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
<br>
<br>

<img width="600" alt="스크린샷 2022-07-22 오후 9 37 13" src="https://user-images.githubusercontent.com/80838501/180440771-240e64fb-c1af-4062-b61b-3ad32d6abe5c.png">

- 서버가 가상호스트를 통해 여러 도메인을 한 번에 처리할 수 있는 서버인 경우, 실제 애플리케이션이 여러 개 구동될 수 있다.
- 이 때, `Host: aaa.com`과 같이 Host 헤더 필드값을 지정해주면 서버가 가상호스팅을 한다.
<br>

### Location
> 페이지 리다이렉션
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
- 201(Created): Location 값은 요청에 의해 생성된 리소스 URI
- 3xx (Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킨다.
<br>

### Allow
> 허용 가능한 HTTP 메소드
```
Allow: GET, HEAD, PUT
```
- 405(Method Not Allowed)에서 응답에 포함해야 하는 헤더
<br>

### Retry-After
> 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
```
Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
Retry-After: 120 (초단위 표기)
```
- 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있다.
<br>
<br>
<br>
<br>

## 인증
```
Authorization: 클라이언트 인증 정보를 서버에 전달
WWW-Authenticate: 리소스 접근 시 필요한 인증 방법 정의
```
<br>

### Authorization
> 클라이언트 인증 정보를 서버에 전달
- Authorization: Basic ---(인증과 관련된 값)
<br>

### WWW-Authenticate
> 리소스 접근 시 필요한 인증 방법 정의
```
WWW-Authenticate: Newauth realm="apps", type=1, title="Login to /"apps/"", Basic realm="simple"
```
- 401 Unauthorized 응답과 함께 사용하는 헤더
<br>
<br>
<br>
<br>

## 쿠키
- 쿠키를 사용할 때는 아래의 두 헤더를 사용
  - **Set-Cookie**: 서버에서 클라이언트로 쿠키 전달(응답)
  - **Cookie**: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달
<br>

### Stateless
- HTTP는 기본적으로 Stateless 프로토콜이기 때문에, 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
- 또한, 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못하고, 클라이언트와 서버는 서로 상태를 유지하지 않는다.
> 따라서, 쿠키를 사용하지 않으면 서버는 이전 요청을 기억하지 못하기 때문에 로그인을 한 후 welcome 페이지에 접근했는데도 서버는 이 클라이언트가<br>
> 회원인지 아닌지 알 수 없는 상황이 발생하게 된다.
<br>
<br>

### 로그인 - 쿠키 사용
> 로그인
<img width="600" alt="스크린샷 2022-07-24 오후 5 28 52" src="https://user-images.githubusercontent.com/80838501/180638957-ee31480f-efd5-4d51-bb7e-a8499ced1519.png">

- `Set-Cookie 헤더`로 user 정보 쿠키 저장소에 저장
<br>

> 로그인 이후 welcome 페이지 접근
<img width="600" alt="스크린샷 2022-07-24 오후 5 29 22" src="https://user-images.githubusercontent.com/80838501/180639237-e4e62ce4-9d3a-4422-a933-8bf0259594b9.png">

- 웹 브라우저는 요청을 보낼 때마다 쿠키 저장소를 뒤져 `Cookie 헤더`로 서버에 보낸다.
- 모든 요청에 쿠키 정보가 자동으로 포함된다.
<br>
<br>

### 쿠키
```
Set-Cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/;domain=.google.com;Secure
```
- 사용자 로그인 세션 관리, 광고 정보 트래킹 등에 사용
- 쿠키 정보는 항상 서버에 전송된다.
  - 네트워크 트래픽 추가 유발하기 때문에, 최소한의 정보만(세션 id, 인증 토큰) 사용해야 한다.
  - 만약 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage, sessionStorage) 사용
<br>
<br>

### 쿠키 - 생명주기
> Expires, max-age
```
Set-Cookie: expires=Sat, 26-Dec-2020 00:00:00 GMT
```
→ 만료일이 되면 쿠키 자동으로 삭제
<br>  

```
Set-Cookie: max-age=3600
```
- 초단위 유효기간 지정. 초과 시 쿠키 자동 삭제
- 0이나 음수 지정하면 쿠키 삭제
- 두 가지 종류의 쿠키 존재
  1) 세션 쿠키: 만료 날짜를 생략하면 브라우저가 종료될 때까지만 유지
  2) 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지
<br>
<br>

### 쿠키 - 도메인
- 도메인을 명시한 경우) 명시한 문서 기준 도메인 + 서브 도메인(하위 도메인) 포함
  - Ex) domain=example.org를 지정해서 쿠키를 생성한 경우 <br>
  → example.org는 물론, dev.example.org(하위 도메인)도 쿠키 접근
- 도메인을 생략한 경우) 현재 문서 기준 도메인만 적용
  - Ex) example.org에서 쿠키를 생성하고 domain 지정 생략한 경우 <br>
  → example.org에서만 쿠키 접근, dev.example.org(하위 도메인)은 쿠키 미접근
<br>
<br>

### 쿠키 - 경로
```
path=/home
```
- 도메인으로 한 번 필터링하고, 경로로 추가 필터링(도메인 안의 경로로)
- 경로를 넣으면, 이 경로를 포함한 하위 경로 페이지만 쿠키 접근 가능
- 일반적으로 `path=/` 루트로 지정
  - 보통 한 도메인 안에서 다 쿠키를 전송하기 원하기 때문
<br>
<br>

### 쿠키 - 보안
> Secure, HttpOnly, SameSite
- Secure
  - 쿠키는 http, https를 구분하지 않고 전송하는데 `Secure` 적용 시 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가하고 HTTP 전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송 가능
