## javascript checklist02

### 6. 비동기 콜백함수와 스택간의 관계를 어떻게 설명할 수 있는지?

- heap은 메모리를 할당하고 스택에 있는 함수를 부른다.
  - web api : Ajax, setTimeOut, event 등을 의미한다. 브라우저에서 지원한다.
  - callback Queue : onClick, Scroll 등 이벤트 함수와 비동기 콜백 함수를 이 queue에 넣는다.
  - Event Loop : 스택과 큐를 보고 있다가 스택이 비어있으면 callback Queue로 부터 스택에 넣는다.

그림

<img src="http://prashantb.me/content/images/2017/01/js_runtime.png" />

예제

<img src="http://cek.io/images/event-loop/loupe.gif" />

> 1. `console.log("hi")`가 callstack에 push 된다.
> 2. `console.log("hi")`는 return 되면서 stack의 맨 위에서 pop 된다.
> 3. `setTimeOut` 함수가 callstack에 push 된다.
> 4. `setTimeOut` 함수는 web api의 부분으로  web api에서 관리 하며 2초동안 처리한다.
> 5. 계속 해서 `console.log("Everybody")` 함수를 callstack에 push 한다.
> 6. `console.log("Everybody")`가 리턴 되고 stack에서 pop 된다.
> 7. 2초가 지난 후 콜백 함수인 cb는 callback queue로 이동 하게 된다.
> 8. event loop는 call stack이 비어있는지 확인하고 만약 비어있지 않다면 기다린다. callstack이 비어 있어야 callback queue에서 callstack에 push 된다.
> 9. callback 내부의 `console.log("There");`도 callstack에 push 되고 return 시 pop 된다.
> 10. 마지막으로 callback 함수인 cb 함수가 pop 된다.



### 23. this 에 대해서 설명하기. 

**설명**

-  `this`는 두가지 조건에 의해 값이 결정 된다.
-  어떤 영역에서 호출되는가?
-  호출자는 누구인가?

-  global context 
~~~javascript
console.log(this.document === document);
console.log(this === window);
this.a = 37;
console.log(window.a);
~~~
위의 예제처럼 global scope 에서 this는 `window`객체를 가리킨다. 하지만 strict 모드일 경우에는 undefined를 가리킨다.

- 함수 호출 패턴
~~~javascript
function func(){
  return this;
}

func() === window;
//true
~~~
위의 예제와 같이 context를 정해 주지 않은 함수안에서의 `this`는 가장 상위의 window 객체를 가리킨다. 이 역시 strict 모드 일 경우에는 undefined를 가리킨다. 함수 호출 패턴 사용시 주의 할 점은 객체의 메소드 내부의 함수에서 `this`를 사용 할경우 context를 지정해 주지 않으면 `this`는 global 객체를 가리키기 때문에 조심해야 한다.

- 메소드 호출 패턴 / 생성자 호출 패턴
~~~javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
var person1 = new Person('dhkim', 29);
console.log(person1.name);
console.log(person1.age);

Person.prototype.isYoung = function() {
  return this.age < 25;
}

console.log(person1.isYoung());
~~~
위의 예제와 같이 객체를 생성하고 메소드를 호출 할때에는 어떤 인스턴스 또는 객체가 호출 했는가가 중요하다. 해당 객체에서 사용한 `this`는 해당 proprty를 호출한 객체나 인스턴스가 된다.

- apply 호출 패턴(Apply Invocation Pattern)
  call, apply, bind 함수등을 통해 함수의 `this` context를 강제적으로 정해주는 것이 가능하다.
~~~javascript
function greet() {
  var reply = [this.person, 'Is An Awesome', this.role].join(' ');
  console.log(reply);
}

var i = {
  person: 'Douglas Crockford', role: 'Javascript Developer'
};

greet.call(i);
~~~
위 예제에서 greet 함수를 호출하 였을때 this는 i 객체. call 메소드를 통해 context를 "i" 객체로 정해 주었기 때문에 호출 시 해당 context가 정해지는 this는 i로 바뀌게 되고 console.log로 제공 되는 결과는 "Douglas Crockford Is An Awesome Javascript Developer" 이다



### 24. this를 변경시킬 수 있는 방법을 코드로 예시로 보여주기. 

- apply, call, bind 예제
  https://github.com/DongHyunKims/javascript-notes/tree/master/notes/bind%2Ccall%2Capply




### 38. preventDefault는 언제 쓸 수 있는 것인지?

**preventDefault**

preventDefault는 현재 이벤트의 기본 동작을 중지 하기위해 사용한다. `a` 태그를 예를 들어 들어 보자.

**html**
~~~html
<div id="div_">
    DIV영역
    <p id="p_">
        P영역
        <a id="a_" href="http://www.google.co.kr" target="_blank">A영역 구글 링크</a>
    </p>
</div>

<section id="console"><br></section>
~~~

**javascript**

~~~Javascript
//DIV 영역에 클릭 이벤트 설정
$("#div_").on("click",function(event){
    $("#console").append("<br>DIV 클릭");
});

//P 영역에 클릭 이벤트 설정
$("#p_").on("click",function(event){
    $("#console").append("<br>P 클릭");
});

//A 영역에 클릭 이벤트 설정
$("#a_").on("click",function(event){
    $("#console").append("<br>A 클릭");
    
    //이벤트의 기본 동작을 중단한다.
    event.preventDefault();
});
~~~
`event.preventDefault();`함수는  `a` 태그의 기본 기능인 `링크 기능`을 사용하지 않고 명시한 이벤트를 적용시킬때 사용 된다.

**stopPropagation**



stopPropagation 함수는 이벤트가 상위 element로 전파되는 것을 막기위한 함수이다.

**html**
~~~Html
<div id="div_">
    DIV영역
    <p id="p_">
        P영역
        <span id="span_">SPAN영역</span>
    </p>
</div>

<section id="console"><br></section>
~~~

**javascript**
~~~javascript
//DIV 영역에 클릭 이벤트 설정
$("#div_").on("click",function(event){
    $("#console").append("<br>DIV 클릭");
});

//P 영역에 클릭 이벤트 설정
$("#p_").on("click",function(event){
    $("#console").append("<br>P 클릭");
});

//SPAN 영역에 클릭 이벤트 설정
$("#span_").on("click",function(event){
    $("#console").append("<br>SPAN 클릭");
    
    //상위로 이벤트가 전파되지 않도록 중단한다.
    event.stopPropagation();
});
~~~
위의 예제에서 처럼 `span` 영역에 이벤트를 적용 시키면 `p`, `div`영역에 이벤트가 전파 되지 않는다.

만약 element의 기본 기능도 없애고 상위 element로 이벤트의 전파를 막고 싶다면, `stopPropagation`, `preventDefault` 함수를 같이 사용 하면 된다.

만약 `stopPropagation`, `preventDefault` 함수를 둘다 사용한 효과를 얻고 싶다면 `   return false;`를 사용하면 된다.

~~~Javascript
//A 영역에 클릭 이벤트 설정
$("#a_").on("click",function(event){
    $("#console").append("<br>A 클릭");
    
    //jQuery 이벤트의 경우,
    //return false는 event.stopPropagation()과 event.preventDefault() 를
    //모두 수행한 것과 같은 결과를 보인다.
    return false;
});
~~~

