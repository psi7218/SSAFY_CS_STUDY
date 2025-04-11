## CORS란? (Cross-Origin Resource Sharing)

`교차 출처 리소스 공유` 이며, 이는 출처가 다른서버의 공유를 허용한다는 뜻입니다.
<br/>
<br/>
<br/>
<br/>
### 2. SOP

SOP는 다른 출처에서 가져온 리소스에 대한 접근을 제한하여 보안을 강화하는 것입니다.

이를 통해 악의적인 코드가 사용자의 개인 정보에 접근하거나 다른 웹 사이트에 영향을 미치는 것을 방지할 수 있습니다.
<br/>
<br/>
<br/>
<br/>
### 3. URL에 대한 기본 개념 및 용어 정리

URL은 *"Uniform Resource Locator"* 의 약자로, 웹 페이지나 파일 등 인터넷 상의 자원의 위치를 가리키는 주소입니다.

간단히 말해, 웹사이트나 파일이 어디에 있는지를 알려주는 것입니다.

<br/>

#### ![이미지9](9_image.png)

| 이름 | 설명 |
| ------ | ------ |
| 프로토콜(Protocol) | URL의 가장 첫 부분으로, 자원에 접근하는 방법을 지정합니다. 일반적으로 "http://" 또는 "https://"와 같이 시작돼요.|
| 도메인(Domain) |  자원이 호스팅되어 있는 서버의 주소를 나타내며, "www.example.com"과 같이 표현돼요.|
| 포트(Port) |  웹 서버에 접속할 때 사용하는 포트 번호를 나타내는 부분으로, 필수적인 경우는 드뭅니다. 일반적으로 "http://"의 경우는 80번 포트, "https://"의 경우는 443번 포트가 사용돼요. |
| 경로(Path) | 서버에서 자원의 위치를 나타내는 부분으로, "example.com/resources/page.html"과 같이 표현돼요.
| 쿼리(Query) |  URL의 끝 부분에 위치하며, 추가적인 매개변수를 전달할 때 사용돼요. 일반적으로 "?"로 시작하고 "&"로 구분돼요. |
|  프래그먼트(Fragment)  |  URL의 일부분으로, 문서나 리소스 내의 특정 부분을 가리킬 때 사용돼요. 주로 "#"으로 시작돼요.  |


<br/>
<br/>
<br/>
<br/>

### 4. SOP를 충족시키기 위한 조건

> 출처를 구성하는 3요소 = 프로토콜, 도메인, 포트가 같아야 합니다.

<br/>

✅ 도메인

www는 서브도메인이기 때문에, 충족요건에는 크게 영향을 미치지 않습니다.

하지만, 도메인 자체는 있는경우와 없는경우 같은 것으로 보지만(뒤에 도메인이 같기만하면) 주소는 다르다고 할 수 있습니다.

<br/>

✅ 포트

HTTP의 기본 포트는 80이고, HTTPS의 기본 포트는 443입니다.

이를 생략하는 것은 일반적으로 가능해요. 하지만 ":8080"과 ":80"은 서로 다른 포트로 간주합니다.

<br/>
<br/>
<br/>
<br/>

### 5. CORS 작동방법 3가지

<br/>

✅ 예비 요청 (Preflight Request)
> 브라우저가 예비요청을 보내는 것을Preflight라고 부르며, 이 예비요청의 HTTP 메소드를 GET이나 POST가 아닌 OPTIONS라는 요청이 사용된다는 것이 특징입니다.

#### ![이미지10](10_image.png)

<br/>
<br/>

✅ 인증된 요청(Credentialed Request)
> 클라이언트에서 서버에게자격 인증 정보(Credential)를 실어 요청할때 사용되는 요청입니다.

여기서 말하는자격 인증 정보란 **세션 ID가 저장되어있는 쿠키(Cookie) 혹은 Authorization 헤더에 설정하는 토큰 값** 등을 일컫습니다. 그래서 주로 로그인이 필요한 경우 많이 사용됩니다.

#### ![이미지11](11_image.png)


<br/>
<br/>

✅ 단순 요청 (Simple Request)
> 심플한 만큼 특정 조건을 만족하는 경우에만 예비 요청을 생략할 수 있습니다.


대표적으로 아래3가지 경우를 만족할때만 가능합니다.
1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 합니다.
2. Accept,Accept-Language,Content-Language,Content-Type,DPR,Downlink,Save-Data,Viewport-Width,Width 헤더일 경우 에만 적용됩니다.
3. Content-Type 헤더가application/x-www-form-urlencoded,multipart/form-data,text/plain중 하나여야한다. 아닐 경우 예비 요청으로 동작합니다.

#### ![이미지12](12_image.png)

> *하지만 실제로 단순요청이 될 가능성은 적다.
왜냐하면 대부분 HTTP API 요청은text/xml 이나application/json 으로 통신하기 때문에 3번째 
Content-Type이 위반되기 때문이다.*
