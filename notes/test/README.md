### 1. 이벤트 델리게이션의 실제원리와 장점을 설명 하시오
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

- 장점 : 필요한 element 전체에 이벤트를 적용 하지 않고 상위의 element에 이벤트를 등록 시키면 하위의 자식 element에서 사용 할수 있는 장점이 있다.

### 2. 프로토타입 메소드와 인스턴스 메소드의 차이점은?



### 3. 비동기 콜백 함수가 call stack 사이에서 어떻게 실행 되는가?

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

### 4. 비동기 함수에서 this 키워드가 어떻게 변경되는지 예를 들어보고 이를 해결하는 방법을 설명하세요.





#### 답

이벤트델리게이션을 통한 이벤트 등록은 여러개의 이벤트리스너를 만들지 않아 메모리 효율성이 있는 장점이 있음.

델리게이션을 이용한 원리는, 버블링(이벤트전파)이라는 특성을 활용한 것이다.

a -> li -> ul > div 와 같은 dom tree가 있을때, a를 클릭하는 순간, 각각 4개의 dom tree에 바인딩된 이벤트리스너가 있는지 탐색하고 있다면

해당 이벤트리스너를 실행하게 된다.  만약 div에만 이벤트리스너를 등록했다고 가정하면, div의 리스너가 실행되고, 그때의 event target은 실제 눌러진 a  태그를 가리킨다. 만약 li를 클릭했다고 해도 마찬가지다. 이때도 li -> ul>div 버블링이 일어나고, div에 바인딩된 이벤트리스너가 실행되며 그때의 event 객체의 target은 li태그를 가리키게 된다. 

이벤트델리게이션은 이벤트의 버블링이라는 특성을 잘 활용한 방법이라고 할 수 있다.

또한 동적으로 추가되는 엘리먼트에 별도의 핸드러를 등록하지 않아도 되는 장점이 있음.

 

그리고 버블링과 캡쳐링은 다름. 보통 버블링이 기본값임으로(addeventlistener의 마지막 인자로 이를 설정가능) 자식노드에서 위쪽으로 탐색이 시작됨.

 

---

prototype객체는 공유된다. 메모리 효율성을 올릴 수 있는 장점이 있다.

prototype이라는 객체는 일종의 상속의 개념과 유사하다고 볼수도 있다.

따라서 인스턴스마다 재사용이 필요한 메서드들은 prototype의 속성으로 추가하는 것이 효율적이다. 

---

bind메서드는 this키워드가 변경되는 것을 방지하는 것이 아니고, this키워드가 가리키는 context를 원하는대로 변경시킬 수 있는 메서드이다.

bind의 첫번째 인자로 원하는 context를 지정하면 된다.

apply와  call은 context를 직시 변경해서 함수를 실행하는 것으로, bind와 완전히 다르게 동작함. 

---

비동기콜백함수들은 stack이 비워져 있을때, stack공간에 올라와서 실행된다.



