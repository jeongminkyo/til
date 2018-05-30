###CROSS DOMAIN

서로 다른 도메인에서 Javascript로 접근하려 했을때 또는 다른 서버에 Ajax통신의 결과를 받는 행위를 말한다.

하지만 위와같은 행위를 하려할때 Javascript의 보안정책중 하나인 Same-Origin Policy(동일 근원 정책)에 걸려서 원하는 결과를 받아낼수 없게 된다.

 

### Same-Origin Policy

다른 도메인의 서버에 요청하는 것을 보안 문제로 간주하고 이를 차단한다. 자바스크립트로 다른 웹페이지에 접근 할려고 하려면 같은 출처의 페이지에만 접근이 가능하다. 같은 출처라는 것은 프로토콜, 호스트명, 포트가 같다는 것을 의미한다. 이것은 [www.ozit.co.kr](http://www.ozit.co.kr/) 도메인에서 호출된 AJAX는 [www.ozit.co.kr](http://www.ozit.co.kr/) 도메인 내에 있는 URL만을 호출할 수 있다는 의미다. 다시말하면 [www.ozit.co.kr](http://www.ozit.co.kr/) 도메인에서 [www.tistory.com](http://www.tistory.com/) 의 URL을 AJAX로 호출할 수 없다. 



### 해결방안

1. JSONP

   `<script/>` 태그는 same-origin-policy (SOP) 정책에 속하지 않는다는 사실을 근거로, 서로 다른 도메인간 javascript 호출을 위하여 jsonp (또는 json with padding) 이 사용되었다.

   JSONP 방식은 XMLHttpRequest 객체를 이용해서 서버로 요청을 전송하는 대신 동적으로 <script> 태그를 만들어 페이지에 삽입한다. 이 때 아래와 같이 script 태그의 src 속성에 호출할 API의 url을 넣어주고 query string에 callback 함수의 이름을 파라미터로 추가한다. 

   ~~~html
   <script src="http://api.example.com/resources?callback=success"></script>
   ~~~

   서버는 요청 받은 작업을 수행한 후에 클라이언트로 응답을 전송한다. 그런데 결과를 그대로 전송하는 것이 아니라, callback 파라미터로 넘어온 콜백 함수를 호출하면서 응답결과를 호출 인자로 전달하는 스크립트 코드를 만들어 클라이언트로 전송한다.

   ~~~javascript
   success({ key : value })   // 응답결과를 인자로 전달하는 콜백함수 호출 코드를 만들어 전송
   ~~~

   클라이언트는 넘어온 결과 값을 해석해서 실행한다. 그러면 결과적으로 success 함수가 호출된다. 이는 Ajax 요청 응답 결과를 success 콜백함수가 받아서 실행하는 것과 같다.

   

   ##### jQuery에서 JSONP 쓰는법

   ~~~javascript
   $.ajax({
       url : "http://other.example.com/resources",
       dataType : "jsonp",
       jsonp : "success",
       success : function(results){
           // success callback
       }
   });
   ~~~

   

   ##### json, jsonp 차이 

   JSONP는 JSON에 패딩(padding)을 더한 것이다. 즉, 앞쪽에 문자열(함수명처럼 사용)을 둔 후 원래 JSON 데이터를 괄호( )로 둘러쌓는 형태가 된다. 예로 들면,

   ~~~json
   //JSON 
   
   {“name”:“stackoverflow”,“id”:5} 
   
   //JSONP 
   
   func({“name”:“stackoverflow”,“id”:5}); 
   
   ~~~

   결과적으로 JSON을 스크립트처럼 부를 수 있게 된다. 위의 예에서처럼 JSONP를 정의해두고 만약 func를 호출하는 함수를 만들어두었다면 JSON 데이터를 하나의 인자로 부를 수 있게 된다.

   

   ##### 단점

   GET Method만을 사용할 수 있다는 점이다. 이것은 JSONP가 <script> 요소를 사용하기 때문에 가지는 숙명적인 한계라고 할 수 있다.



2. CORS

   웹 브라우저엥서 외부 도메인 서버와 통신하기 위한 방식을 표준화한 스펙이다. 서버와 클라이언트가 정해진 헤더를 통해 서로 요청이나 응답에 반응할지 결정하는 방식으로 교차 출처 자원 공유(cross-origin resource sharing)라는 이름으로 표준화가 되었다.

   CORS를 사용하기 위해서 클라이언트와 서버는 몇 가지 추가 정보를 주고 받아야 한다. 클라이언트는 CORS 요청을 위해 새로운 HTTP 헤더를 추가한다. 서버는 클라이언트가 전송한 헤더를 확인해서 요청을 허용할지 말지를 결정한다.데이터에 사이드 이펙트를 일으킬 수 있는 HTTP 메소드를 사용할 때는 먼저 preflight 요청을 서버로 전송해서 서버가 허용하는 메소드 목록을 HTTP OPTIONS 헤더로 획득한 다음에 실제 요청을 전송한다.

   

   **Simple Requests**

   기존 데이터에 사이드 이펙트를 일으키지 않는 GET, HEAD, POST 요청을 Simple Request라고 한다. POST 요청의 경우에는 서버로 전송하는 Content-Type이 application/x-www-form-urlencoded, multipart/form-data, text/plain 중에 하나여야 한다. 이 때는 HTTP 요청에 커스텀 헤더를 지정하지 않는다.

   simpleRequest 함수를 호출하면 클라이언트는 아래의 요청을 서버로 전송한다.

   ~~~
   [Request Header]
   
   GET /resources/users/ HTTP/1.1
   Host: api.com
   User-Agent:
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
   Accept-Language: en-us,en;q=0.5
   Accept-Encoding: gzip,deflate
   Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
   Connection: keep-alive
   Referer: http://example.com/index.html
   Origin: http://example.com
   ~~~

    `Origin:` 헤더가 해당 요청이 "`http://example.com`" 도메인에 있는 컨텐츠로부터 온 것이라는 것을 알려주는 중요한 HTTP 요청 헤더이다.

   ~~~
   [Response Header]
   
   HTTP/1.1 200 OK
   Date: Mon, 01 Dec 2008 00:23:53 GMT
   Server: Apache/2.0.61
   Access-Control-Allow-Origin: *
   Keep-Alive: timeout=2, max=100
   Connection: Keep-Alive
   Transfer-Encoding: chunked
   Content-Type: application/json
   ~~~

    응답에서 보면, 서버는 `Access-Control-Allow-Origin:` 헤더를 전달하고 있다. 위 경우에, 서버는 리소스가 cross-site 방식 내에서 **모든** 도메인으로부터 접근 가능하다는 것을 의미하는 "`Access-Control-Allow-Origin: *`"와 함께 응답하고 있다. 만약 리소스에 대한 접근을 `http://foo.example`에게만 허용하길 바란다면, 다음과 같이 응답해야 한다.

   `Access-Control-Allow-Origin: http://foo.example`

   

   **Preflighted Requests**

   GET, HEAD, POST 이외의 메소드를 이용한 요청은 데이터에 사이드 이펙트를 만들 수 있기 때문에, CORS 스펙에 따라 클라이언트는 preflight 요청을 먼저 서버로 전송해야 한다. POST 요청이지만 Content-Type이  application/x-www-form-urlencoded, multipart/form-data, text/plain이 아닌 경우도 여기에 해당한다. 이 때는 커스텀 헤더를 설정한다.

   브라우저는 실제 요청을 전송하기 전에 OPTIONS 메소드를 이용해서 preflight 요청을 서버로 전송한다.

   ~~~
   [Request Header]
   
   OPTIONS /resources/post-here/ HTTP/1.1
   Host: api.com
   User-Agent:
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
   Accept-Language: en-us,en;q=0.5
   Accept-Encoding: gzip,deflate
   Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
   Connection: keep-alive
   Origin: http://example.com
   Access-Control-Request-Method: POST
   Access-Control-Request-Headers: X-Custom-Header
   ~~~

   단순 요청에는 없었던 Access-Control-Request-Method, Access-Control-Request-Headers가 추가된 것을 볼 수 있다.

   - Access-Control-Request-Method : 실제 요청 때 사용할 메소드
   - Access-Control-Request-Headers : 브라우저가 실제 요청을 보낼 때 헤더에 추가할 커스텀 속성

   preflight 요청을 받은 서버는 CORS 관련 정보를 헤더에 담아서 아래와 같은 응답을 클라이언트로 전송한다.

   ~~~
   [Response Header]
   
   HTTP/1.1 200 OK
   Date: Mon, 01 Dec 2008 01:15:39 GMT
   Server: Apache/2.0.61 (Unix)
   Access-Control-Allow-Origin: http://example.com
   Access-Control-Allow-Methods: POST, GET, OPTIONS
   Access-Control-Allow-Headers: X-Custom-Header
   Access-Control-Max-Age: 1728000
   Vary: Accept-Encoding
   Content-Encoding: gzip
   Content-Length: 0
   Keep-Alive: timeout=2, max=100
   Connection: Keep-Alive
   Content-Type: text/plain
   ~~~

   여기에서는 Access-Control-… 값을 눈여겨 봐야한다. 각 값은 서버가 허용하는 요청의 범위를 나타낸다.

   - Access-Control-Allow-Origin : 허용하는 Origin
   - Access-Control-Request-Methods : 허용하는 요청 메소드
   - Access-Control-Allow-Headers : 허용하는 헤더 속성
   - Access-Control-Max-Age : preflight request 캐시하는 시간(초 단위)
   - Access-Control-Allow-Credentials : 클라이언트 요청이 쿠키를 통해서 자격 증명을 해야 하는 경우에 true. true를 응답받은 클라이언트는 실제 요청 시 서버에서 정의된 규격의 인증값이 담긴 쿠키를 같이 보내야 한다.
   - Access-Control-Allow-Methods : 요청을 허용하는 메소드. 기본값은 GET, POST라고 보면 된다. 이 헤더가 없으면 GET, POST 요청만 가능하다. 만약 이 헤더가 지정이 되어 있으면, 클라이언트에서는 헤더값에 해당하는 메서드일 경우에만 실제 요청을 시도하게 한다.

   

   **withCredential**

   표준 CORS는 기본적으로 요청을 보낼 때 쿠키를 전송하지 않는다. 쿠키를 요청에 포함하고 싶다면 XMLHttpRequest 객체의 withCredentials 프로퍼티 값을 true로 설정해준다.

   그리고 서버 측도 반드시 Access-Control-Allow-Credentials 응답 헤더를 true로 설정해야 한다.

   

   ##### rails cors

   Rack-cors gem이 존재.(https://github.com/cyu/rack-cors)

   config/application.rb 파일을 수정.

   ~~~Ruby
   module YourApp
     class Application < Rails::Application
       # ...
       
       # Rails 5
   
       config.middleware.insert_before 0, Rack::Cors do
         allow do
           origins '*'
           resource '*', headers: :any, methods: [:get, :post, :options]
         end
       end
   
       # Rails 3/4
   
       config.middleware.insert_before 0, "Rack::Cors" do
         allow do
           origins '*'
           resource '*', headers: :any, methods: [:get, :post, :options]
         end
       end
     end
   end
   ~~~

   

 ##### 



[출처] 

http://dvlp.tistory.com/7

http://ooz.co.kr/232

http://enterkey.tistory.com/409

https://blog.seotory.com/post/2016/04/understand-jsonp

http://dev.epiloum.net/1311

https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS

https://brunch.co.kr/@adrenalinee31/1

http://wit.nts-corp.com/2014/04/22/1400