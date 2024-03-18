# Origin(출처)
> URL 상의 Protocol과 Host 그리고 Port를 Origin이라고 한다.
![origin](https://github.com/AucSuSu/CS-study/assets/139415941/b154ef7b-eb16-451e-b95f-9e6d2f026d36)

<br>

## 동일 출처 정책(Same-Origin Policy)
 - 동일 출처 정책(Same-origin policy)는 다른 출처로부터 조회된 자원들의 읽기 접근을 막아 다른 출처 공격을 예방한다.

 - 동일출처 정책은 다른 출처 자원을 가져오는 것을 굉장히 제한적으로 
 허용한다.

 - 다른 출처 리소스에 접근성을 높이기 위해서 CORS가 등장했다.

> Tip. 출처 비교와 차단은 서버가 아닌 브라우저가 한다.

<br>

## 교차 출처 리소스 공유 (Cross-Origin Resource Sharing)

<br>

 - 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)는 단어 그대로 다른 출처의 리소스 공유에 대한 허용/비허용 정책이다.

 - SOP의 예외 사항을 두기 위해 CORS 정책을 허용하는 리소스에 한해 다른 출처라도 받아들인다는 것이다.

<br>

### CORS 기본 동작
1. 클라이언트에서 HTTP요청의 헤더에 Origin을 담아 전달

2. 서버는 응답헤더에 Access-Control-Allow-Origin을 담아 클라이언트로 전달

3. 클라이언트에서 Origin과 서버가 보내준 Access-Control-Allow-Origin을 비교

<br>

### CROS의 종류

<br>

**1. 프리 플라이트(Preflight Request)**

 - 프리 플라이트는 OPTIONS 메서드로 HTTP 요청을 미리 보내 실제 요청이 전송하기에 안전한지 확인한다.

 - 다른 출처 요청이 유저 데이터에 영향을 줄 수 있기 때문에  미리 전송한다는 의미다.

 - 이때 브라우저가 예비요청을 보내는 것을 Preflight라고 부른다.

 - 이 예비요청의 HTTP 메소드를 GET이나 POST가 아닌 OPTIONS라는 요청이 사용된다는 것이 특징이다.

![preflightRequest](https://github.com/AucSuSu/CS-study/assets/139415941/9827620e-1c63-4bdc-bd45-dd7806aa4bfc)

<br>

**2. 단순요청(Simple Request)**

 - 단순 요청은 말그대로 예비 요청(Prefilght)을 생략하고 바로 서버에 직행으로 본 요청을 보낸 후, 서버가 이에 대한 응답의 헤더에 Access-Control-Allow-Origin 헤더를 보내주면 브라우저가 CORS정책 위반 여부를 검사하는 방식이다.

- GET, HEAD, POST 요청만 가능하다.

- Content-Type 헤더가 application/x-www-form-urlencoded, multipart/form-data, text/plain중 하나여야한다. 아닐 경우 예비 요청으로 동작된다.

- 대부분의 HTTP API 요청은 application/json으로 통신하기에 위의 조건에 위반된다.

<br>

**3. 신용 요청(Credentialed Request)**

- 신용 요청은 쿠키, 인증 헤더, TLS 클라이언트 인증서 등의 신용정보와 함께 요청한다.

- 기본적으로, CORS 정책은 다른 출처 요청에 인증정보 포함을 허용하지 않는다.
 
- 요청에 인증을 포함하는 플래그가 있거나 access-control-allow-credentials가 true로 설정 한다면 요청할 수 있다.


- 여기서 말하는 자격 인증 정보란 세션 ID가 저장되어있는 쿠키(Cookie) 혹은 Authorization 헤더에 설정하는 토큰 값 등을 일컫는다.

- 서버도 마찬가지로 이러한 인증된 요청에 대해 일반적인 CORS 요청과는 다르게 대응해줘야 한다.

- 즉, 응답의 Access-Control-Allow-Origin 헤더가 와일드카드(*)가 아닌 분명한 Origin으로 설정되어야 하고, Access-Control-Allow-Credentials 헤더는 true로 설정되어야 한다는 뜻이다. 그렇지 않으면 브라우저의 CORS 정책에 의해 응답이 거부된다.