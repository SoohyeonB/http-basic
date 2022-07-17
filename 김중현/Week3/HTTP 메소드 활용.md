# Section 5. HTTP 메소드 활용
## 클라이언트에서 서버로 데이터 전송
> 데이터 전달 방식은 크게 두 가지
- **쿼리 파라미터**를 통한 데이터 전송
  - GET에서 많이 사용
  - 주로 정렬 필터(검색)를 쓸 때 많이 사용
- **메시지 바디**를 통한 데이터 전송
  - POST, PUT, PATCH에서 사용
  - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경할 때 주로 사용
<br>
<br>

> 4가지 상황에서의 데이터 전송
- 정적 데이터 조회
  - 이미지, 정적 텍스트 문서
- 동적 데이터 조회
  - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
- HTML Form을 통한 데이터 전송
  - 회원 가입, 상품 주문, 데이터 변경
- HTTP API를 통한 데이터 전송
  - 회원 가입, 상품 주문, 데이터 변경
  - 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)
<br>
<br>

### 1) 정적 데이터 조회
> 쿼리 파라미터 미사용
- 이미지, 정적 텍스트 문서
- 조회는 GET 사용
- 정적 데이터는 쿼리 파라미터 없이 리소스 경로만으로 조회 가능 (추가 데이터 필요X)
<br>
<br>

### 2) 동적 데이터 조회
> 쿼리 파라미터 사용
```
GET /search?q=hello&hl=ko HTTP/1.1 
Host:www.google.com
```
- 서버가 쿼리 파라미터를 기반으로 정렬 필터해 결과를 동적으로 생성
<br>

- 주로 검색, 게시판 목록에서의 정렬 필터(검색어)
- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
- 조회는 GET
- GET은 쿼리 파라미터를 사용해 데이터 전달
<br>
<br>

### 3) HTML Form 데이터 전송
> POST 전송 - 저장, GET 전송 - 저장
```html
<form action="/save" method="post">
    <input type="text" name="username" />
    <input type="text" name="age" />
    <button type="submit">전송</button>
</form>
```
- submit 버튼이 눌리면 웹 브라우저가 form의 데이터를 읽어서 HTTP 메시지를 생성해준다.
<br>

**웹 브라우저가 생성한 요청 HTTP 메시지**
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded

username=kim&age=20
```
- 쿼리 파라미터와 거의 유사한 형태로 데이터를 만들어 HTTP 바디에 넣는다. 
<br>
<br>

- HTML Form으로 데이터 전송할 때, method를 GET으로 바꿀 수도 있다.
  - POST면 데이터를 메시지 바디에 넣고, GET이면 아래와 같이 url 경로에 넣는다.
```
GET /save?username=kim&age=20 HTTP/1.1 
Host:localhost:8080
```
<br>
<br>

> multipart/form-data
```html
<form action="/save" method="post" enctype="multipart/form-data">
    <input type="text" name="username" />
    <input type="text" name="age" />
    <input type="file" name="file1" />
    <button type="submit">전송</button>
</form>
```
- 여러 content type에 대한 데이터를 보낼 수 있게 해준다.
- 주로 바이너리 데이터를 전송할 때 사용
<br>
<br>

### 4) HTTP API 데이터 전송
- 서버 to 서버
  - 백엔드 시스템 통신 시 많이 사용
- 앱 클라이언트에서 전송할 때도 많이 사용
- 웹 클라이언트
  - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  - Ex) React, VueJS 같은 웹 클라이언트와 API 통신 시 사용
- POST, PUT, PATCH 사용 시 메시지 바디를 통해 데이터 전송
- GET 사용 시 쿼리 파라미터로 데이터 전달
- Content-Type: application/json을 주로 사용 (XML이 읽기 쉽지 않고 데이터도 복잡하기 때문에 심플하고 데이터 크기도 작은 json 사용)
