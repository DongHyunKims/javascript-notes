#Scope(유효범위)

## 설명
- 프로그래밍 언어에서 scope은 변수와 매개변수의 생존 기간을 제어.
- scope는 이름이 충돌하는 문제를 줄여주고, 자동으로 메모리를 관리 하기 때문에 프로그래머에게 중요한 개념.
- 본래 자바스크립트는 block scope(scope 종류 참조) 이 없다. 자바스크립트에서는 function scope이 존재.
- 자바스트립트에서는 함수에서 사용하는 모든 변수를 함수 첫 부분에서 선언 하는 것이 최선의 방법.
- 유효범위 개념을 잘 알고 있다면 변수와 매개변수의 접근성과 생존기간을 제어할 수 있다.

## scope 의 종류
#### 1. Global Scope
- 함수 밖에서 정의 된 scope.
- 웹페이지의 모든 script와 function 에서 접근 할수 있는 영역

**그림1**
<img src="http://www.datchley.name/content/images/2015/08/js-es5-scope-2.png" />

**예제1**
~~~javascript
var carName = "Volvo";
//Volvo
// code here can use carName
function myFunction() {
    carName = "MyCar"
    //MyCar
}
 carName = "NiceCar"
//NiceCar
~~~
위 예제에서 Global Scope 선언된 `var carName`는 어디서든 접근이 가능하다. 그렇기 때문에
`myFunction` 함수안에서도 `carName` 변수를 접근할수 있으며 밖에서도 접근이 가능하다.

#### 2. Function scope/ Local Scope
- function 내부에서 정의 된 변수는 function 외부에서 접근이 불가능하다.
- function 내부에서 정의 된 변수는 function 내부의 어디든 접근이 가능하다.
- 지역변수는 함수안에서만 인정되며 다른 함수에서는 같은 이름의 변수를 사용 할수 있다.(javascript는 같은 함수안에서도 사용 가능함)

**예제2**
~~~javascript
// code here can not use carName
function myFunction() {
    var carName = "Volvo";
    // code here can use carName
}
carName = "NiceCar"
~~~
위 예제에서 function scope 안에서 선언된 `var carName`는 선언된 함수 내에서만 사용이 가능하다. 그렇게 함수 외부에서 `carName`에 접근 할 시 function 내부에 있는 `carName`를 접근 하는 것이 아닌 `window.carName`의 property에 "NiceCar"를 할당 한다. 


## javascript 유효범위의 특징
javascript의 유효 범위는 다른 프로그래밍 언어와 다른 개념을 갖는다.

#### 1. 함수 단위의 유효범위

**예제3**
~~~Javascript
function test() {  
 if(true){
   	var a = 1;
    console.log("a1=" + a);
 }
  console.log("a2=" + a);
}

//console.log("a3=" + a);
test();  
/*
결과1
a1=1
a2=1
	
결과2(console.log("a3=" + a); 구문 실행시)
a is not defined
*/
~~~
보통 프로그래밍 언어는 block 단위의 scope를 가지고 있어서 if/for문 등의 `{}`안에서 사용한 변수는 외부에서 접근이 불가능 하다. 하지만 자바스크립트는 함수를 제외한 모든 블럭에서 접근이 가능하다. 위 예제에서 if구문 안에 선언된 **a**를 if의 block 내부와 외부에서 모두 사용 가능하다. 하지만 test 함수 밖에서 a를 접근 하려고 하면 함수 안에 있는 a를 접근 하지 못하고 `a is not defined`이라는 오류 구문을 제공한다.

#### 2. 변수명 중복 허용

**예제4**
~~~Javascript
function test() {  
  var a = 1;
  var a = 3;
  console.log("a=" + a);
}
test();  
/*
결과
a=3
*/
~~~
보통 c유형에 기반을 둔 프로그래밍 언어에서는 변수명의 중복을 허용 하지 않는다. 하지만 자바스립트에서는 마지막에 선언한 변수명의 값을 사용한다. 위의 예제에서 a를 중복으로 정의 해도 마지막에 에러가 제공되지 않고 마지막에 할당한 3을 사용한다. 즉 변수 참조 시 에 가장 가까운 범위의 변수를 참조한다. 

#### 3. var 키워드의 생략

**예제5**
~~~Javascript
function test1(){  
    a = 20;
    console.log("a1 = " + a);
}
function test2(){  
    console.log("a2 = " + a);
}
console.log("a3 = " + a);
test1();  
test2();  
/*
a1 = 20 
a2 = 20 
a3 = 20
*/
~~~
일반 적인 프로그래밍 언어에서는 데이터 형식 키워드를 생략 할수 없다. 하지만 자바스크립트에서는 데이터 타입 작성을 생략 할 수 있다. 데이터 타입을 작성하지 않고 정의한 변수는 Global scope에 존재 한다. 위 예제에서 선언된 a 변수는 Global scope에 존재 하기 때문에 어느곳에서든 접근이 가능하다.

#### 4. 렉시컬 특성
- 렉시컬 특성이란 함수 실행 시의 scope을 실행 할때가 아닌 정의 했을때의 상황으로 참조.
- 함수가 정의 될 때 자신의 유효범위를 저장

**예제6**
~~~Javascript
function test1(){  
    var a= 10;
    test2();
}
function test2(){  
    return a;
}
test1();
/*
결과
a is not defined
*/
~~~
위 예제에서 전역 변수에 선언된 test1, test2는 어디에서든지 사용 할수 있다. 하지만 test1실행시에 a가 정의 되있다고 하더라고 선언시 에는  Global 또는 function 에서 a가 정의 되어있지 않기 때문에 a에 접근 할수 없다.


## scope chain
- Scope들이 chain 처럼 연결되어있다.
- 식별자(identifiers)를 찾는 일련의 과정
- scope chain은 function이 정의된 장소에서부터 시작. 어디서 호출 되었는지 와는 전혀 상관 없다.

**그림2**
  <img src="http://figures.oreilly.com/tagoreillycom20090601oreillybooks300541I_book_d1e1/figs/I_mediaobject7_d1e7080-web.png" />


**예제7**
~~~Javascript
var returns_a_func = function () {
  var number = 5
  return function(){ console.log( "The number is: " + number) }
}

var number = 4
var fn = returns_a_func()
fn() // what will this log? why?

var func_runner = function(func){
  var number = 3
  func()
}

func_runner(fn) // what will this log? why?

/*
결과
The number is: 5
The number is: 5
*/
~~~
위의 예제가 잘 이해가 안될수 있다. 처음 부터 결과를 보지 말고 예제를 이해해 보길 바란다. 위 예제에서 `returns_a_func`함수가 정의 되어있고 전역 변수로 `var number = 4`가 정의 되어있다. 그리고 `fn` 변수 에는 `returns_a_func`함수 실행시 리턴되는 `function(){ console.log( "The number is: " + number) }` 함수가 할당된다. 이때 fn을 실행 시키면 fn 함수 안에 number가 없더라도 scope chain을 통해 fn가 정의 되었던 returns_a_func을 탐색 하게 된다. 해당 함수안에 정의된 `var number = 5`가 있으므로 fn 함수에서 number 해당 number을 사용해 `The number is: 5` 출력이 가능하다. 이후 `func_runner`함수를 정의하고 실행시 전달인자로 `fn`함수를 전달해 준다. `func_runner`가 실행하면서 전달인자로 받았던 `fn`를 실행 시키게 되는데 이 경우에는 자바스크립트의 렉시컬적인 특성에 의해 정의할때 결정된 scope의 number값을 사용하여 결과는 `The number is: 5`가 제공 된다. 



## Tip
- block scope 
- `{}`으로 표현 하며 block 밖에서 block 안 쪽에 있는 변수를 접근 할수 없다. 안쪽에서 밖으로의 접근은 가능하다.

## 참고
- https://www.w3schools.com/js/js_scope.asp
- http://www.nextree.co.kr/p7363/
- 자바스크립트 핵심 가이드(더글라스 크락포드 저)