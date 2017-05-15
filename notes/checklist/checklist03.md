## javascript checklist03

#### 7.  DOM 조작과정에서 성능에 좋지 않은 방식과 좋은 사례는 어떤 것들이 있는지?
- 리플로우 : 생성된 DOM node의 레이아웃(너비, 높이 등) 변경시, 영향 받는 모든 노드(자식, 부모 등) 모든 수치를 다시 계산하여 트리를 재생성 하는 방법.
~~~javascript
function reflow(){
  document.getElementById("test").style.width = "100px";
  return false;
}
~~~

- 리페인팅 - 리플로우과정이 끝난 후 재 생성된 렌더트리를 다시 그리는 작업. 수치와 상관없는 background-color, visibility, outline등의 스타일 변경시에는 reflow 작업이 생략된 repaint 작업이 일어난다.
~~~javascript
function repaint(){
	document.getElementById("test").style.backgroundColor = "red";
	return false;
}
~~~

리플로우와 리페인팅이 자주 일어날수록 부하가 높아진다.

##### 좋지 않은 예

~~~javascript
var console = document.getElementById("console");
var lis = document.getElementsByTagName("li");
var len = lis.length;
for(var i = 0; i < len; i++) {
    console.innerHTML += li[i].innerHTML + "<br />";
}
~~~

##### 좋은 예
~~~javascript
var console = document.getElementById("console");
var lis = document.getElementsByTagName("li");
var len = lis.length;
var cache = [];
for(var i = 0; i < len; i++) 
    // 일반적으로 이러한 문자열 결합은 +로 더해가는 것보다,
    // 배열을 사용하여 전부 요소로 등록하고 join을 쓰는 것이 빠르다.
    cache.push(li[i].innerHTML);
}
console.innerHTML = cache.join("<br />");
~~~



querySelect 는 굉장히 부하가 많이 걸린다 그렇기 때문에 namespace 방식을  통해서 한번만 queryselect 또는 elementByid 를 통해 element를 가져오고 사용하는 것이 좋다.





**참조**

- https://github.com/nhnent/fe.javascript/wiki/Reflow%EC%99%80-Repaint
- http://webclub.tistory.com/346
- https://lists.w3.org/Archives/Public/public-html-ig-ko/2011Sep/att-0031/Reflow_____________________________Tip.pdf

#### 9.  prototype 이 가진 장점은 무엇인가? 

**설명**
- prototype의 장점은 공통으로 사용하는 메소드를 모두 모아 놓고, 공유 하여 사용 할 수 있다.
- 효율성과 재사용성에 관한 문제이다. 만약 배열 생성자 함수가 만드는 배열 인스턴스 마다 `join()`과 같이 항상 똑같이 동작하는 메소드를 굳이 일일이 추가할 필요가 있을까? 하나의 `join()`을 모든 배열에서 가져다 쓰는 편이 더 합리 적이다.
- 모든 객체는 속성을 상속하는 프로토 타입과 연결되어있다. 생성자 함수의 프로토 타입 속성을 통해 공통의 메소드와 속성을 공유하고 상속한다. 인스턴스 메소드는 인스턴스가 생성 될때마다 메모리를 차지하지만 프로토 타입 메소드는 한번만 차지하고 해당 메소드를 모든 인스턴스와 객체가 공유 하기 때문에 메모리에 효율적이며 재사용성 높였다.

그림 

<img src="http://poiemaweb.com/img/object_literal_prototype_chaining.png" />

예제
~~~javascript
let myArr = new Array("foo","bar");
console.log(myArr.join());
~~~
위 예제에서 Array의 `join()` 메소드는 myArr 인스턴스에 정의 되지 않은 메소드이다. `join()` 메소드는 프로토 타입에 정의가 되어있기 때문에 Array객체의 모든 인스턴스는 해당 메소드를 사용할수 있다. 모든 배열 인스턴스마다 함수를 새로 만드는 것보다 하나의 join() 함수를 모든 배열에서 가져다 사용 하는 것이 효율적이기 때문이다.

#### 10.  prototype 을 통해 객체를 만드는 방법에 대해서 설명하기.



#### 12.  event delegation의 원리는 무엇인지? 적용한 경험이 있다면 어떤기능이였는지?
**설명**
- Event delegation을 사용 하면 부모 element에 적용한 이벤트는 자식 element에도 모두 적용 된다. 
- Event propagation : Event delegation을 이해하기 위한 중요한 요인 중 하나이다. Event propagation 은 자식 element에서 이벤트가 발생하면 연쇄적으로 부모와 조상 element에도 이벤트가 발생 한다. 즉, a 태그를 하나 클릭 하게 되면 전체 document body에 효과적인 clicking을 할수 있도록 한다. 
    - `<a>`
    - `<li>`
    - `<ul #list>`
    - `<div #container>`
    - `<body>`
    - `<html>`
    - `document root`
- Event delegation은 Event propagation의 방식을 참조하여 이벤트가 발생한 element보다 상위 레벨에서 이벤트를 처리하는 것을 의미한다.
- 특정 element 에서 event가 발생하면, 해당 event 는 모든 상위 element로 전달되며(bubbling phase), 각 element 에 등록된 모든 event handler 가 호출된다

html
~~~html
<ul id="parent-list">
<li id="post-1">Item 1</li>
<li id="post-2">Item 2</li>
<li id="post-3">Item 3</li>
<li id="post-4">Item 4</li>
<li id="post-5">Item 5</li>
<li id="post-6">Item 6</li>
</ul>
~~~

javascript
~~~javascript
// Get the element, add a click listener...
document.getElementById("parent-list").addEventListener("click", function(e) {
// e.target is the clicked element!
// If it was a list item
if(e.target && e.target.nodeName == "LI") {
    // List item found!  Output the ID!
    console.log("List item ", e.target.id.replace("post-"), " was clicked!");
       }
 });
~~~

- 장점 : 필요한 element 전체에 이벤트를 적용 하지 않고 상위의 element에 이벤트를 등록 시키면 하위의 자식 element에서 사용 할수 있는 장점이 있다. 또한 이벤트 등록을 위해 여러개의 이벤트리스너를 만들지 않아 메모리 효율성이 있는 장점이 있다.

- 사용 이유 : 

  **1) 동일 element 유형들에 대해 같은 event handler 를 등록해야 할 때**

  ex) `<div id="tour"></div>` 아래 `<button></button> `이 100개 있다고 하면, 100개의 event handler를 등록해야 한다. 성능에 악 영향을 주고, 소스도 길어진다.

  **2) call back 함수, timer에 의해 element 가 생길 경우**

  call back 함수 실행 후 또는 특정 시간 후 element 가 생기기 때문에 element 생기기 전 직접 event handler를 등록할 수 없다. delegation 의 경우 상위 element 에 event handler를 등록하기 때문에, delegation 을 통해서라면 가능하다.

#### 56.  javascript의 call stack에 대해서 설명하세요. 

**설명**

javascript 에서 call stack은 interpreter(web browser 내부에 있는 interpreter) 하나의 매커니즘 이다. 여러개의 함수가 불렸을때  해당 함수의 위치를 tracking 할 수 있는 매커니즘이다.

1. script가 함수를 호출하면 interpreter는 이를 호출 스택에 추가 한 함수를 수행하기 시작한다.
2. 해당 함수 내부에서 호출되는 모든 함수들도 전부 call stack에 추가 된며 도달위치에서 실행된다.
3. 메인 함수가 종료 될때 interpreter는 stack을 모두 비우고 main code listing에서 중단 된 실행을 다시 시작한다.
4. 스택이 할당 된 것보다 많은 공간을 차지하면 "stack overflow" 오류가 발생한다.

**그림**

<img src="https://www.simple-talk.com/wp-content/uploads/2016/09/ProcessFlow.png" />

Tip

**컴파일러**

고급언어로 쓰여진 프로그램이 컴퓨터에서 수행되기 위해서는 컴퓨터가 직접 이해할 수 있는 기계어로 바꾸어 준다. 이러한 일을 하는 프로그램을 컴파일러라고 한다. 번역과 실행 과정을 거쳐야 하기 때문에 번역 과정이 번거롭고 번역 시간이 오래 걸리지만, 한번 번역한 후에는 다시 번역하지 않으므로 실행 속도가 빠르다.

**인터프리터**

소스 프로그램을 한번에 기계어로 변환시키는 컴파일러와는 달리 프로그램을 한 단계씩 기계어로 해석하여 실행하는 ‘언어처리 프로그램’이다. 줄 단위로 번역, 실행되기 때문에 시분할 시스템에 유용하며 원시 프로그램의 변화에 대한 반응이 빠르다. 한 단계씩 테스트와 수정을 하면서 진행시켜 나가는 대화형 언어에 적합하지만, 실행 시간이 길어 속도가 늦다는 단점이 있다.


**그림**

<img src="http://cfile3.uf.tistory.com/image/147FE84F4F2E9E100E47E7" />


#### 55.  http header는 무엇인가요?

1. HTTP request header

웹브라우저가 HTTP프로토콜을 이용해 요청 정보를 웹 서버로 전송할 때 HTTP 요청 헤더에 부가적인 정보(user-agent, host 등)를 담아 전송한다. 

- accept : 클라이언트가 처리하는 미디어 타입 명시 (예 : */*)
- accept-language : 클라이언트가 지원하는 언어 지정 (예 : ko)
- accept-encoding : 클라이언트가 해석할 수 있는 인코딩 방식 지정(예 : gzip, deflate)
- user-agent : 클라이언트 프로그램(브라우저) 정보 (예 : Mozilla/4.0 (compatible; - MSIE 6.0; Windows NT 5.1))
- host : 호스트 이름과 URI의 port번호 지정 (www.test.com:8080)
- connection : 클라이언트와 서버의 연결 방식 설정(Keep-Alive : 클라이언트와 접속 유지,close : 클라이언트와 접속 중단)
- cookie : 웹서버가 클라이언트에 쿠키를 저장한 경우 쿠키 정보(이름,값)을 웹 서버에 전송 예 : JSESSIONID=CDEI3830DJEJ3K3KD23K39D49)



2. HTTP Response Header

서버가 HTTP프로토콜을 이용해 클라이언트의 요청에 대해 HTML문서를 전송할 때 부가적 정보(content-type, content-length, server 등)를 HTTP 응답 헤더에 담아 함께 전송하게 된다. 

- connection : 클라이언트와 서버의 연결 방식 설정(Keep-Alive : 클라이언트와 접속 유지,close : 클라이언트와 접속 중단)
- Content-Type : 헤더 응답 문서의 mime 타입
- Content-Length : 요청한 파일의 데이터의 길이
- Last-Modified : 문서가 마지막으로 수정된 일시
- Server :  웹서버 정보
- Date : 현재 일시를 GMT 형식으로 지정
- ETag : 캐시 업데이트 정보를 위한 임의의 식별 숫자

**출처**

http://appsnuri.tistory.com/61



#### 54.  post와 get의 차이점을 설명하세요.
**설명**

**1. 일반 적인 GET과 POST의 차이**

1. GET 
- 전송할 데이터를 URL에 담아 보내는 방식.
- URL의 길이 제한이 있어 많은 양의 데이터를 보내기 어렵다.
- 보안에 가장 취약 하다. 데이터가 URL에 보이기 때문에.
- 접근한것이 모두 캐시가 된다.
2. POST
- 전송한 데이터를 body에 넣어서 보내는 방식.
- GET 방식보다 데이터를 담을수 있는 용량이 많아 대용량 데이터를 보내기에 적합하다.
- 데이터가 외부로 노출 되어있는 것이 아니기 때문에 GET 방식 보다 안전하지만 완전히 안전한 것은 아니다.

**2. GET과 POST는 언제 사용해야하는가?**

**GET은 가져오는 것이고 POST는 수행하는 것.**

1. GET
- GET은 select 적인 성향을 가진다.
- 예를 들어 GET은 서버를 통해 어떠한 데이터를 검색 해서 보여 주는 용도이며, 서버의 값이나 상태등을 바꾸지는 않는다. 게시판의 리스트를 보여주거나 상세글을 보여주는 용도로 사용한다.
- 웹에서 모든 리소스는 Link할 수 있는 URL을 가지고 있어야 한다. 어떤 페이지를 보고 있을때 다른 사람한테 그 주소를 주기 위해서 주소창의 URL을 복사해서 줄 수 있어야 한다. POST를 할 결우에는 값이 내부적으로 전달되기 때문에 URL만 전달할수 없는 단점이 있다. 글을 저장하는 경우에는 URL을 제공할 필요가 없기 때문에 POST를 해도 상관이 없다.

2. POST 
- POST는 서버를 통해 값이나 상태를 바꾸기 위해 많이 사용된다.
- 예를 들어 글쓰기를 통해 글의 내용을 DB에 저장 하거나 수정을 하면 DB의 값이 수정이 된다. 이럴 경우 POST를 사용합니다. 

**3. 사례**

Google의 Accelerator

Accelerator라는 것은 그이름대로 웹서핑의 속도를 향상시킬 목적으로 구글이 발표한 프로그램이다. 어떤 웹사이트에 갔을때 페이지에 있는 URL등을 Accelerator가 미리 모두 클릭해봐서 사용자가 해당 URL로 이동하기 전에 이미지등의 미리 받아놓을 수 있는 것들을 받아놓아 웹서핑의 체감속도를 높여주는 것이 목적이었다. 캐시때문에 한번 방문한 사이트는 더 빨리 뜨는 것을 이용.

하지만 실제 많은 개발자들은 GET과 POST를 용도구분없이 혼용해서 사용했고 Delete같은 곳에도 GET방식을 편의대로 이용해 버렸다. Accelerator는 이것을 구분하지 못하니 URL만 보였다 싶으면  클릭을 해댄 것이고 Bot이 직접 URL로 접근해버리자 해당 데이터들은 Delete를 수행해버리면서 메일이나 게시글이 지워지는 사태가 발생했다.

**4. 참조**
- https://blog.outsider.ne.kr/312
