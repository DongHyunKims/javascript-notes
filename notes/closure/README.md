## Closure

### 설명
- outter 함수 안에 inner 함수 가 포함되어있다면 inner 함수에서 outter함수의 parameter와 변수들을 접근 할수 있다.(this / argument는 예외)
- inner 함수가 outter 함수가 더 오래 남아있는 경우가 특별하다. 
- inner함수를 위한 outter함수의 지역변수 또는 parameter가  outter함수에 의해 inner함수가 반환된 이후에도 life-cycle이 유지되는 것을 의미한다. 
- outter 함수는 inner함수 안에서 정의된 변수와 함수를 접근 하지 못하게 되어있고 이는 inner함수의 변수를 보호한다.
- outter함수의 scope 밖에서 inner함수를 사용하면 cloure scope 이 사용된다.
- 스코프 체인이 클로저를 만든다.


### 중첩된 함수와 클로저
- 중첩된 함수 : 외부 함수 안에 내부 함수가 존재 하는 것, Closure scope을 생성한다.
- inner 함수를 형성 하는 클로저 : outter 함수는 inner 함수의 인수와 변수를 사용 할수 없는 반면에 inner 함수는 outter 함수의 arguments와 변수를 접근할 수 있다.

**예제1**
~~~javascript
function addSquares(a,b) {
  function square(x) {
    return x * x;
  }
  return square(a) + square(b);
}
a = addSquares(2,3); // returns 13
b = addSquares(3,4); // returns 25
c = addSquares(4,5); // returns 41
~~~
위 예제에서 보면 `addSquares`에서 paramater값을 받아 inner 함수인 `square`함수에 paramater를 전달해 주어 실행 시키는 것이 가능하다. 


### 변수를 private 하게 사용
- 중첩된 inner 함수가 반환 될 때 outter 함수의 인수가 보존된다. 
- inner 함수의 메모리는 그 무엇도 내부 함수에 접근 하지 않을때만 해제된다.


**예제2**
~~~javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
~~~
위의 예제에서 `inside`함수는 `outside`함수의 인자인 x를 참조 하고 있다. `outside` 함수가 `inside`함수를 반환 하면서 `outside`함수의 생명 주기는 끝났지만 `inside`함수에서는 x에 대한 참조를 계속 사용 한다. 그리고 x의 값을 외부에서 더이상 접근 하지 못하는 private 상태를 만들었다.

**예제3**
~~~javascript
let out = function(){
let value = 0;
  return function () {
   value++;
   console.log("value",value);
  }
}
let inner = out();

inner();
inner();
~~~
위 예제에서 `out`함수를 호출하면 `out`함수의 `value`변수를 사용하는 함수가 inner 변수에 저장된다. 그리고 inner함수를 호출하면 closure scope을 생성하여 value 변수를 참조 한다. 이 value 변수는 외부에서 참조할수 없는 private한 속성을 가진다.

그림

<img src="../../images/closure.png" />


### 외부함수가 실행 될때마다 새로운 클로저 생성
- 클로저 scope은 외부 함수가 실행 되고 해당 함수 내부에 있는 내부 함수가 실행 될때 마다 새롭게 생긴다.

**예제4**
~~~javascript
let out = function(){
let value = 0;
  return function () {
   value++;
   console.log("value",value);
  }
}
let inner1 = out();

inner1();
inner1();

let inner2 = out();

inner2();
inner2();

/*
결과
value 1
value 2
value 1
value 2
*/
~~~
위 예제는 예제3과 같은 형태로 이루어져 있다. 외부함수인 `out()`함수를 한번 더 실행하여 inner2 변수에 inner 함수를 할당하고 실행 시키면 새로운 클로저 환경의 value를 접근하여 다시 0부터 계산한다.

### 루프에서 클로저 생성하기
- 일반 적으로 loop에서 closure 사용시 많은 실수를 한다. 

**예제5**
html
~~~Html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
~~~

javascript
~~~javascript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}
setupHelp();
~~~
위의 예제는 우리가 loop에서 클로저 사용 시 발생하는 일반적인 실수 이다. `setupHelp()` 함수를 실행 시키면 loop안에 존재하는 `item`은 helpText[2]의 값이 할당 된 채로 함수가 종료된다. 이 후 `focus` 이벤트가 발생할때 마다 outter 함수의 `item`값을 참조하게 되는데 item에는 helpText[2]의 값이 할당 되어있기 때문에 3가지 input 모두 같은 `help`메세지가 제공 된다. 현재 3개의 이벤트가 모두 같은 클로저를 공유 하고 있기 때문이다.

**해결방법1**
~~~javascript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function makeHelpCallback(help) {
  return function() {
    showHelp(help);
  };
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}

setupHelp();
~~~
예제5의 실수를 해결하기 위해 위의 방법을 사용한다.  `makeHelpCallback`함수를 이용하여 각각의 이벤트 마다 새로운 클로저 환경을 만들어 주면 해결이 가능하다. loop를 돌면서`makeHelpCallback`함수를 실행시키면 makeHelpCallback에 의해 익명 함수 return 된다. 해당 익명 함수는 이벤트가 발생 할때 마다 실행 된다. 이때 익명 함수가 참조 하고 있는 `help` 변수는 outter 함수인 `makeHelpCallback`의 arguments인 help를 참조하고 있으므로 정상적인 결과를 제공한다. 3개의 이벤트 모두 다른 클로저 환경을 참조하는 것이다.

**해결방법2**
~~~
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    let item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
~~~
예제5의 문제를 해결하는 2번째 방법은 `var` 대신 `let` 사용하는 것이다. 


### 더 조사할 점
- javascript의 객체는 함수이고 객체지향을 위해 클로저가 생겼다?
- 렉시컬 스코핑

### 참고
- 자바스크립트 핵심 가이드(더글라스 크락포드 저)
- javascript MDN(closure) - https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures