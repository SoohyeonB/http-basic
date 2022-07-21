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
 
