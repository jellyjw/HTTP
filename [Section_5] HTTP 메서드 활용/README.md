## Client에서 Server로 데이터 전송

전송할때는 크게 2가지 방법으로 나뉜다.

- query-parameter 를 통한 데이터 전송
- 메세지 바디를 통한 데이터 전송
  - POST, PUT, PATCH …
  - 회원가입, 상품주문, 리소스 등록, 변경 등에 사용

### 1) 정적 데이터 조회

예를 들어 이미지나, 정적 텍스트 문서를 조회하는 경우에는 쿼리파라미터를 사용하지 않고 단순 url만 알아도 `GET` 메서드로 조회가 가능하다.

- GET 메서드
- 쿼리 파라미터 미사용

### 2) 동적 데이터 조회

동적 데이터는 말그대로 동적으로 변화하는 데이터를 조회할때 사용되므로, 쿼리파라미터를 사용해서 조회할수 있다. 주로 검색이나 게시판 목록에서 정렬 필터(검색어)를 할 때 정렬 조건에 사용된다.

- GET 메서드
- 쿼리 파라미터 사용

```jsx
https://www.google.com/search?q=hello&hl=ko
```

사용자가 검색 결과를 ‘hello’ 로 입력했을 경우 `q=hello` 라는 쿼리파라미터로 서버에 어떤 검색어를 입력했는지 전송하는 것이다. 만약 ‘bye’ 를 입력하면 `q=bye` 라고 동적으로 value가 변하고 이를 통해 동적 데이터를 조회할수 있다.

### 3) HTML FORM 데이터 전송

![image](https://github.com/jellyjw/HTTP/assets/104891203/a8996041-711b-4002-ba06-789d9f1eb1a2)

어떠한 데이터를 전송할때 `form` 태그를 사용해서 전송하게 되는데, 해당 태그로 입력값을 받은 뒤 submit을 하게 되면 HTTP 메시지를 생성해준다.

주의할 점은, 해당 메서드가 `POST` 라는 점이며 `GET` 은 조회에만 사용하기 때문에 리소스가 변경되는 곳에 사용하면 안된다는 점이다.

- HTML Form 에서 submit시 POST 메서드로 메세지가 생성되며 서버로 전송된다.
- Content-Type : acclication/x-www-form-urlencoded 사용
  - 전송 데이터를 URL encoding 처리
    - ex) abc김 → abc%EA%B9%80
  - form의 내용을 http 메시지 바디를 통해 전송(key=value, 쿼리파라미터 형식으로)
- 파일 업로드같은 바이너리 데이터 전송도 가능
  - Content-Type: multipart/form-data
- HTML Form 전송은 **GET, POST** 만 지원한다.

### 4) HTTP API 데이터 전송

우리가 흔히 사용하는 JSON 타입으로 데이터를 전송하는것을 `API` 데이터 전송이라고 한다.

- 주로 서버 to 서버일때 사용
- 앱, 웹 client
  - HTML에서 Form 전송 대신 JS를 통한 통신에 사용(AJAX)
  - React, Vue.js와 같은 웹 client와 API 통신
- Content-Type: application/json을 주로 사용 (사실상 표준)

---

## HTTP API 설계 예시

### 1) POST 기반 등록

POST의 특징은 **클라이언트가 등록될 리소스의 URI를 모른다** 는 것이다.

members라는 url에 요청을 보내면, 서버에서 리소스 디렉토리를 관리하여 새로운 URI를 생성하고 관리한다.

여기서 `/members` 는 **컬렉션** 이다. 클라이언트에서 요청을 보내면, 서버에서 응답으로 members의 100번에 회원이 등록되었다는 메세지를 보내준다.

- 회원 등록 `/members`
- POST `/members`

### 2) PUT 기반 등록

PUT의 특징은 **클라이언트가 등록될 리소스의 URI를 알고있다** 는 것이다.

이는 직접 클라이언트가 리소스의 URI를 지정하기 때문에, 클라이언트가 리소스를 관리한다는것과 같으며

이를 **스토어** 라고 한다.

- Store
  - 클라이언트가 관리하는 리소스 저장소
  - URI를 관리
- PUT `/members/{memberId}`

### 3) HTML Form 사용

HTML Form은 GET, POST만 지원한다. AJAX 같은 기술을 사용해서 해결은 가능하지만 순수 HTML Form은 GET과 POST만 지원하기 때문에 여러 제약이 따른다.

예를 들어, Delete의 경우 아래와 같이 요청을 보내면 쉽게 해당 멤버의 정보 삭제가 가능할 것이다.

- DELETE `/members/{id}`

하지만 HTML Form은 GET, POST만 지원하므로 **컨트롤 URI** 를 사용하여 해결해야 한다.

- POST `/members/{id}/delete`
  → path에 delete라는 동사로 구분해줬다. 이를 컨트롤 URI라고 한다.

### 컨트롤 URI

실무에서 HTTP 메서드로 전부 처리하기에는 어려움이 있고, 애매한 상황이 생긴다. 이때 컨트롤 URI로 동사로 된 리소스 경로를 사용하여 해결할 수 있다.

- /new
- /edit
- /delete

---

### 정리

1. POST와 PUT의 차이는 다음과 같다.

- URI 리소스를 클라이언트가 알고있냐 / 모르고 있냐 ?
- URI 리소스를 클라이언트에서 생성, 관리(PUT, `store`) / 서버에서 생성, 관리(POST, `Collection`)

1. PUT은 교체의 개념인데 만약 회원 정보를 수정할 경우 전화번호만 수정하더라도 나머지 정보를 모두 보내야 하기 때문에, PATCH를 사용하는게 가급적 좋다. 하지만, 게시글 수정같은 경우는 수정버튼을 누르고 모든 정보를 수정하고 보내기 때문에 이 때에는 PUT을 사용하는게 좋다.
2. 보통 애매한 경우에는 천하무적인 POST를 쓰자.
3. 실무에서 URI 설계할 때 애매한 경우가 많은데, 이때는 컨트롤 URI를 떠올려 사용하자.
