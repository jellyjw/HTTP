## URI(Uniform Resource Idenrifier)

![image](https://github.com/jellyjw/HTTP/assets/104891203/579128c9-84e4-46b2-a44d-7d13c316f720)


URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있는, URL과 URN을 포함하는 개념이다.

리소스(자원) 자체를 식별하는 방법이다.

- Uniform : 리소스를 식별하는 통일된 방식
- Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음, ex: html 파일 뿐만 아니라 실시간 교통정보 등)
- Identifier : 다른 항목과 구분하는데 필요한 정보

![image](https://github.com/jellyjw/HTTP/assets/104891203/a32d99d5-7917-4c3d-a02c-4b18e1ab964a)


### URL(Uniform Resource Locator)

Locator, 리소스가 있는 **위치**를 나타낸다.

```jsx
https://www.google.com/search?q=hello&hl=ko
```

우리가 실제로 브라우저 주소창에 입력하는게 URL이다. 다음과 같이 이루어져 있다.

- 프로토콜(protocol) : https, http, ftp 등
  - 어떤 방식으로 자원에 접근할것인가를 정하는 약속 규칙이다.
- userInfo : `scheme://[userinfo@]`
  - URL에 사용자정보를 포함해서 인증하는 방법인데, 거의 사용하지 않는다.
  - 나는 깃허브 저장소에 접근하지 못할때 내 github 아이디를 이런식으로 붙여서 자주 사용했다.
- 호스트명(host) : [www.google.com](http://www.google.com)
  - 도메인명 또는 IP주소를 직접 사용 가능하다.
- 포트 번호(port) : https - 443, http - 80 등
  - 일반적으로 생략되며 접속 포트를 의미한다.
- 패스(path) : `/search`
  - `home/file1.jpg` `members/100` 등 계층적 구조로 이루어져 있다.
  - 계층적 구조로 설계해야 접근도 쉽고, 알아보기도 쉽다.
- 쿼리(query)
  - `?q=hello&hl=ko`
  - ?로 시작해서 &로 추가가 가능하다.
  - key=value 형태이다.
  - 웹서버에 제공하는 파라미터라는 의미로 `query parameter` 라고도 불린다.
  - 어떤식으로 입력해도 `string` 으로 전송되기 때문에 `query string` 이라고도 불린다.
- fragment
  - url 뒤에 붙는 `#fragment` 형태
  - html 내부 북마크 등에 사용한다.
  - 서버에 전송되는 정보는 아니다.

### URN(Uniform Resource Name)

말그대로 리소스에 이름을 부여한 것으로 URL이 위치를 나타낸다면 URN은 리소스의 이름 그 자체이다.

위치는 변할 수 있지만 이름은 변하지 않기 때문에, `URN` 이름만으로 실제 리소스를 찾을수 있는 방법이 보편화 되지 않아 잘 사용되지 않는다.

---

## 웹 브라우저 요청 흐름

주소창에 다음과 같은 URL을 입력하면 어떻게 될까? 웹 브라우저의 요청 흐름을 알아보자.

```jsx
https:www.google.com:443/search?q=hello&hl=ko
```

1. 먼저 DNS 서버에서 host(`[www.google.com](http://www.google.com)`)를 조회해서 IP를 받아온다.
2. HTTP 요청 메세지를 생성한다.

![image](https://github.com/jellyjw/HTTP/assets/104891203/9d96f5c6-e55f-4817-86ca-9d47f383b54f)


3. HTTP 메시지를 전송한다.
   1. 웹 브라우저가 HTTP 메시지 생성
   2. SOCKET 라이브러리를 통해 전달(TCP/IP 연결, 데이터 전달)
   3. TCP/IP 패킷 생성, HTTP 메시지 포함
4. 패킷 생성
   1. 출발지 IP, PORT
   2. 목적지 IP, PORT
   3. 전송 데이터 등을 포함
5. 브라우저에서 구글 서버로 요청 패킷 전달
6. 구글 서버에서 요청 패킷의 메세지를 해석하고, 응답 메세지를 생성한다.

![image](https://github.com/jellyjw/HTTP/assets/104891203/52afe122-0116-474e-bed6-4f4f21853f20)


7. 구글 서버에서 응답 패킷을 웹 브라우저에 전달한다.
8. 브라우저에서 응답을 받고 HTTP 메세지(html 파일)를 브라우저에 렌더링한다.
