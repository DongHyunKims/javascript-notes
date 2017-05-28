## RESTful API

- REST : REPresentational State Transfer
- 어떻게 전송을 할것인가? 
- 데이터를 주고 받을때 나아진 방법을 이야기 한다.
- URI를 만들때 정해진 규칙.


1. RESTful API
- REST한 방식의 API.
- 웹을 근간으로 하는 HTTP Protocol 기반.
- resource는 URI로 푠현 하며, 말 그대로 "고유" 해야한다.
- URI는 단순하고 직관적인 구조. : 어떤 자원을 의미하는 지 명확해야 한다.
- 리소스의 상태(여러가지 상태)는 HTTP Methods 활용해서 구분.
- 동사형태의 주소보다는 명사형태이며 Methods에 행위를 작성. 
- xml/json을 사용하여 데이터 전송. (주로 json)

2. CRUD
- 네트워크를 통해 웹 리소스를 다루기 위한 행위.
- 각각의 행위를 처리하기 위한 HTTP methods(POST, GET, PUT, DELETE) 가 존재
- CREATE(POST), RETRIEVE(GET), UPDATE(PUT), DELETE(DELETE)

3. API Design
- 주로 복수 명사를 많이 사용(/movies, /cars)
- 필요하면 URL에 하위자원 표현 (/movies/23) : 23번째 영화
- 필터조건을 허용할수 있음 (/movies?state=active) : movie 아래 상태가 active인것!
- PUT, DELETE는 AJAX 통신에 의해서만 가능하다.

<img src="https://i.stack.imgur.com/BHweq.png" />


4. 모바일 환경

접속하는 환경에 따라 다른 정보를 보여주어야 할때가 있다. 예를 들어 `m.naver.com` ,`www.naver.com` 과 같이 하드웨어에 따라 보여지는 화면이 다르다. RESTful 하게 작성 하려면 `URI는 중립적`이어야 한다. 하나의 URI 를 클릭해도 하드웨어에 맞추어 redirect를  다르게 해주는 것이다. req header의 user-agent를 통해 하드웨어를 구분하여 redirect를 해준다.

~~~
Mozilla/5.0 (iPhone; U; CPU like Mac OS X; en) AppleWebKit/420+ (KHTML, like Gecko) Version/3.0 Mobile/1A543a Safari/419.3
~~~

대부분 브라우저, OS 플랫폼은 HTTP Request를 보낼 시 보내는 주체에 대한 설명을 User-Agent에 상세하게 포함하여 통신하고 있기 때문에 요청자의 환경을 정확히 알 수 있다.


4. GET
~~~Javascript
router.get("/movies",(req,res)=>{

})


router.get("/movies/:title",(req,res)=>{

})
~~~


5. POST
~~~Javascript
router.get("/movies",(req,res)=>{

})
~~~

6. DELETE
~~~Javascript
router.delete("/movies",(req,res)=>{

})
~~~

6. PUT
~~~Javascript
router.put("/movies",(req,res)=>{

})
~~~

**참조**

- http://blog.remotty.com/blog/2014/01/28/lets-study-rest/
- https://spoqa.github.io/2012/02/27/rest-introduction.html