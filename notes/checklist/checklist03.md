## javascript checklist03

#### 7.  DOM 조작과정에서 성능에 좋지 않은 방식과 좋은 사례는 어떤 것들이 있는지?
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
#### 54.  post와 get의 차이점을 설명하세요.