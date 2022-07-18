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
