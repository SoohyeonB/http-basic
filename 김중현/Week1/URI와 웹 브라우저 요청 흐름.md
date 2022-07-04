# Section 2. URI와 웹 브라우저 요청 흐름
## URI (Uniform Resource Identifier)
> “URI는 locater, name 또는 둘 다 추가로 분류될 수 있다.”
<img width="465" alt="스크린샷 2022-07-03 오전 12 13 39" src="https://user-images.githubusercontent.com/80838501/177122935-b752fdf1-d01e-4399-af96-b645886e8890.png">

### URI
- **U**niform: 리소스를 식별하는 통일된 방식
- **R**esource: URI로 식별할 수 있는 모든 것
- **I**dentifier: 다른 항목과 구분하는 데 필요한 정보
<br>

- URL: Uniform Resource **Locator**
    - resource가 있는 위치를 지정
- URN: Uniform Resource **Name**
    - resource에 이름을 부여
    - URN 이름만으로 실제 resource를 찾을 수 있는 방법이 보편화되지 않았기 때문에 앞으로 **URL**만 따진다.
<br>
<br>

### URL
```markdown
scheme://[userinfo@]host[:port][/path][?query][#fragment]
```
```markdown
https://www.google.com:443/search?q=hello&hl=ko
```
<br>

### scheme
```markdown
**https**://www.google.com:443/search?q=hello&hl=ko
```
- 주로 프로토콜 사용
    - 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 규칙
    - http, https, ftp
<br>

참고)
- http는 80포트, https는 443 포트 주로 사용 (생략 가능)
- https = http에 보안 추가 (HTTP Secure)
<br>
<br>

### userinfo
- URL에 사용자 정보를 포함해서 인증해야 될 때 사용
- 거의 사용하지 않는다.
<br>
<br>

### host
```markdown
https://**www.google.com**:443/search?q=hello&hl=ko
```
- 호스트명
- 도메인명 또는 IP 주소 직접 사용 가능
<br>
<br>

### PORT
```markdown
https://www.google.com:**443**/search?q=hello&hl=ko
```
- 생략 가능
- 접속 포트
- 일반적으로 생략하고, http는 80, https는 443으로 이해하면 된다.
<br>
<br>

### path
```markdown
https://www.google.com:443/**search**?q=hello&hl=ko
```
- 이 resource가 있는 경로
- 계층적 구조
    - /home/file1.jpg
    - /members/100
<br>
<br>

### query
```markdown
https://www.google.com:443/search**?q=hello&hl=ko**
```
→ q에 넣은 값을 검색, hl에 넣은 언어로 데이터를 내려준다.

- `key=value` 형태
- `?`로 시작하고 `&`로 파라미터를 추가로 붙일 수 있다.
    - ?keyA=valueA&keyB=valueB
- query parameter 또는 query string으로 불린다.
<br>
<br>

### fragment
```markdown
https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html**#getting-started-introducing-spring-boot**
```
- html 내부 어느 위치 (북마크)로 이동하고 싶을 때 사용
- 서버에 전송하는 정보는 아니다.
<br>
<br>
<br>

## 웹 브라우저 요청 흐름
```markdown
https://**www.google.com:443**/search?q=hello&hl=ko
```
1. **DNS 조회**
    - DNS를 조회해 IP 주소를 얻는다.
    - PORT 정보를 찾아낸다.
<br>
<br>

2. **HTTP 요청 메시지 생성**
<img width="443" alt="스크린샷 2022-07-03 오전 12 52 23" src="https://user-images.githubusercontent.com/80838501/177125692-83596ec0-4792-4a2f-a3d3-b8137db80e9c.png">

→ GET   path   query   HTTP 버전 정보   Host 정보
<br>
<br>


3. **HTTP 메시지 전송**
<img width="639" alt="스크린샷 2022-07-03 오전 12 55 13" src="https://user-images.githubusercontent.com/80838501/177125865-09384bb9-3b56-4a2b-a6db-155c9d6b50d8.png">

- 웹 브라우저가 생성한 HTTP 메시지의 겉에 TCP, IP에 대한 정보를 씌워 인터넷으로 흘러들어간다.
<br>
<br>

4. **HTTP 요청 패킷 전달 및 도착**
<img width="577" alt="스크린샷 2022-07-03 오전 12 58 18" src="https://user-images.githubusercontent.com/80838501/177125991-f97d7bd8-c12e-4bda-8c8d-220aa9c13d06.png">

<img width="576" alt="스크린샷 2022-07-03 오전 12 58 40" src="https://user-images.githubusercontent.com/80838501/177126035-166b9a3e-5b85-4d94-a61a-8a4fb60587f5.png">

- 전송한 요청 패킷이 도착하면 TCP, IP 패킷을 버리고 HTTP 메시지를 해석해 요청 조건에 맞는 데이터를 서치한다.
<br>
<br>

5. **HTTP 응답 메시지 생성, 전달 및 도착**
<img width="389" alt="스크린샷 2022-07-03 오전 1 02 56" src="https://user-images.githubusercontent.com/80838501/177127513-0667acfe-89f8-4d4a-a097-8ea42c73b2cc.png">

- Google이 HTTP 응답 메시지를 생성
<br>
<br>

<img width="579" alt="스크린샷 2022-07-03 오전 1 04 45" src="https://user-images.githubusercontent.com/80838501/177127647-55a2ddad-f826-44a6-975a-87908d77db29.png">

<img width="579" alt="스크린샷 2022-07-03 오전 1 05 04" src="https://user-images.githubusercontent.com/80838501/177127682-c4d47ab9-aad0-4d1e-aa43-b4bc50fb294b.png">

- 앞과 동일한 방식으로 응답 패킷을 만들고 TCP, IP 패킷으로 씌워 전달
<br>
<br>

6. **웹 브라우저 HTML 렌더링**
- 도착한 응답 패킷을 까서 HTML 렌더링
