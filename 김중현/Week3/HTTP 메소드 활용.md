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
<br>
<br>
<br>
<br>

## HTTP API 설계 예시
### 1) HTTP API - 컬렉션
> POST 기반 등록
#### 회원 관리 시스템 API 설계
```
회원 목록 /members → GET
회원 등록 /members → POST
회원 조회 /members/{id} → GET
회원 수정 /members/{id} → PATCH, PUT, POST
회원 삭제 /members/{id} → DELETE
```
<br>

#### POST - 신규 자원 등록 특징
- 클라이언트는 등록될 리소스의 URI를 모른다.
  - 회원 데이터를 서버로 보내면 서버가 데이터를 저장하고 회원 ID를 새로 만들어 회원을 식별할 수 있는 URI를 만들어주는 것
  - 회원 등록 /members → POST
  - POST /members
- **서버**가 새로 등록된 리소스 URI를 생성해준다.
  - HTTP/1.1 201 Created <br>
    Location: /members/100
- `컬렉션(Collection)`: 서버가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리한다.
  - 위 예시에서 컬렉션은 /members
<br>
<br>

### 2) HTTP API - 스토어
> PUT 기반 등록
#### 파일 관리 시스템 API 설계 
```
파일 목록 /files → GET
파일 조회 /files/{filename} → GET
파일 등록 /files/{filename} → PUT
파일 삭제 /files/{filename} → DELETE
파일 대량 등록 /files → POST
```
<br>

#### PUT - 신규 자원 등록 특징
- **클라이언트**가 리소스 URI를 알고 있어야 한다.
  - 파일 등록 /files/{filename} → PUT
  - PUT /files/star.jpg
- **클라이언트**가 직접 리소스의 URI를 지정한다.
- `스토어(Store)`: 클라이언트가 관리하는 리소스 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 위 예시에서 스토어는 /files
<br>
<br>

#### 참고
```markdown
대부분 컬렉션 기반의 신규 자원 등록을 쓴다.(POST)
```
<br>
<br>
<br>
<br>

### 3) HTML FORM 사용
> 웹 페이지 회원 관리
- HTML FORM은 **GET, POST**만 지원 <br>
→ AJAX같은 기술을 사용해 해결 가능
- 여기서는 순수 HTML, HTML FORM을 기반으로 설명
<br>

#### HTML FORM API 설계
```
회원 목록 /members → GET
회원 등록 폼 /members/new → GET
회원 등록 /members/new 또는 /members → POST
회원 조회 /members/{id} → GET
회원 수정 폼 /members/{id}/edit → GET
회원 수정 /members/{id}/edit 또는 /members/{id} → POST
회원 삭제 /members/{id}/delete → POST
```
- 컨트롤 URI
  - GET, POST만 지원하므로 제약이 있다.
  - 이러한 제약을 해결하기 위해 **동사로 된 리소스 경로**를 사용
  - **POST의 /new, /edit, /delete**
  - 최대한 리소스 개념을 가지고 설계하고, 안될 때 컨트롤 URI를 사용
  - HTTP 메소드로 해결하기 애매한 경우 사용(HTTP API 포함)
<br>
<br>

#### URI 설계 개념 정리
> https://restfulapi.net/resource-naming
- 문서(document)
  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  - ex) /members/100, /files/star.jpg
- 컬렉션(collection)
  - **서버**가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리
  - ex) /members
- 스토어(store)
  - **클라이언트**가 관리하는 자원 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - ex) /files
- 컨트롤러(controller), 컨트롤 URI
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  - **동사** 사용
  - ex) /members/{id}/delete
