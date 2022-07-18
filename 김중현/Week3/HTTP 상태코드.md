# Section 6. HTTP 상태코드
## HTTP 상태코드 소개
> 클라이언트가 보낸 요청의 처리 상태를 응답(response)에서 알려주는 기능
```
1xx(Information): 요청이 수신되어 처리중
2xx(Successful): 요청 정상처리
3xx(Redirection): 요청을 완료하려면 추가 행동 필요
4xx(Client Error): 클라이언트 오류. 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
5xx(Server Error): 서버 오류. 서버가 정상 요청을 처리하지 못함
```
<br>

#### 모르는 상태코드가 반환될 경우
- 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면 클라이언트는 상위 상태코드로 해석해 처리한다.
- Ex)
  - 299 → 2xx(Successful)
  - 451 → 4xx(Client Error)
  - 599 → 5xx(Server Error) 
- 미래에 새로운 상태코드가 추가되어도 클라이언트를 변경하지 않아도 된다.
<br>
<br>
<br>
<br>

## 2xx - 성공
> 클라이언트의 요청을 성공적으로 처리
```
200 OK
201 Created
202 Accepted
204 No Content
```
### 200 OK
> 요청 성공

<img width="500" alt="스크린샷 2022-07-18 오후 4 34 49" src="https://user-images.githubusercontent.com/80838501/179464234-1a5ff2c6-d6a4-49c8-9a73-8141252b266a.png">

<br>

### 201 Created
> 요청 성공해 새로운 리소스가 생성

<img width="600" alt="스크린샷 2022-07-18 오후 4 35 22" src="https://user-images.githubusercontent.com/80838501/179464439-40167336-df99-4f74-87f2-1ff27b02e487.png">

- 서버가 생성한 URI를 응답의 헤더 필드에 넣어준다.
- 클라이언트가 201 상태코드를 보고 200 시리즈이므로 성공했다고 인식하고, 201이므로 새로운 리소스가 생성됐고 location 헤더가 있을 수 있겠다 생각
<br>

### 202 Accepted
> 요청이 접수되었으나 처리가 완료되지 않았음
- 배치 처리 같은 곳에 사용
- Ex) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리
<br>

### 204 No Content
> 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없는 경우
- Ex) 웹 문서 편집기에서의 save 버튼
- save 버튼의 결과로 아무 내용이 없어도 되고, 버튼을 눌러도 같은 화면을 유지해야 하는 상황
- 결과 내용이 없어도 204 상태코드만으로 성공이라는 것은 인식 가능
<br>
<br>
<br>
<br>

## 3xx - Redirection
> 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
```
300 Multiple Choices
301 Moved Permanetly
302 Found
303 See Other
304 Not Modified
307 Temporary redirect
308 Permanent Redirect
```
### Redirection
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동한다.
<br>

### Redirection 예제
> 전에 이벤트 페이지를 /event로 만들어 쓰다가 더 이상 쓰지 않고 새로운 이벤트 페이지인 /new-event를 쓰는 경우
1. 기존 링크를 아는 사용자들은 `/event`로 들어오려고 한다.
2. 서버가 `/event`는 더 이상 쓰지 않는다는 것을 **301** 상태코드를 통해 클라이언트에 알린다. <br> 
   이때, Location 헤더에 새로운 페이지인 `/new-event`를 넣어 전달한다.
3. Location 경로로 자동 redirect하고, 새로운 경로로 서버에 다시 요청한다.
4. 서버는 응답 HTML을 클라이언트에 내려준다. 
<br>

### Redirection의 종류
- 영구 리다이렉션: 특정 리소스의 URI가 영구적으로 이동
  - Ex) /members → /users
- 일시 리다이렉션: 일시적인 변경
  - 주문 완료 후 주문 내역 화면으로 일시적으로 이동
  - PRG: Post/Redirect/Get
- 특수 리다이렉션
  - 결과 대신 캐시를 사용해도 된다.
<br>
<br>

### 영구 리다이렉션
> 301, 308
- 리소스의 URI가 영구적으로 이동했다는 것을 의미
- 원래의 URI를 사용하면 안되며, 검색 엔진 등에서도 변경을 인지할 수 있다.
1) 301 - Moved Permanently
<img width="600" alt="스크린샷 2022-07-18 오후 5 07 40" src="https://user-images.githubusercontent.com/80838501/179469780-caba795d-9551-4feb-8e4c-cc023f67c528.png">

- 리다이렉트 시, 요청 메소드가 **GET**으로 바뀌고, 본문이 제거될 수 있다.(브라우저들이 거의 이렇게 구현되어 있다.)
- POST를 원했는데 `/new-event`에 대한 GET 요청으로 바뀌고 본문이 제거되며 새로운 이벤트 페이지가 떠서, 등록하려던 것을 처음부터 다시 입력해야 한다.
<br>
<br>

2) 308 - Permanent Redirect
<img width="600" alt="스크린샷 2022-07-18 오후 5 08 17" src="https://user-images.githubusercontent.com/80838501/179470340-c29d4235-46bb-4557-9017-6e23c790ac97.png">

- 301과 기능은 같다.
- 리다이렉트 시, 요청 메소드와 본문을 **유지**한다.(처음 POST를 보내면 리다이렉트도 POST)
- 실무에서 거의 사용하지 않는다. `/event`에서 `/new-event`로 페이지가 바뀌면 내부적으로 전달해야 할 데이터도 다 바뀌어버리기 때문이다.
