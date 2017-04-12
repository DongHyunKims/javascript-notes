## this


### 설명
- 자바스크립트에서 함수의 this 키워드는 다른 언어와 다르게 동작한다. 
- 모든 함수는 이미 작성된 argument 외에 `this` 와 `aruments`라는 추가적인 매개변수 2개를 더 받게 된다. 
- this 키워드는 두가지 조건에 의해 값이 결정 된다.
  - 어떤 영역에서 호출 되는가?
  - 호출자는 누구인가?
- this의 값은 함수를 호출하는 방법에 의해 결정 된다. 실행 하는 동안 결정 되는 것이 아닌 함수가 호출 될때 마다 다르게 결정된다.
- this의 값은 호출하는 패턴에 의해 결정. (함수  / 메소드 / 생성자 / apply 호출 패턴 )


### 전역 컨텍스트
- 가장 상위의 영역
- 함수의 외부에서는 this는 전역 객체(부라우저 : window)를 나타낸다.

예제1
~~~javascript
console.log(this.document === document);
console.log(this === window);
this.a = 37;
console.log(window.a);

/*
결과
true
true
37
*/
~~~
위 예제에서 this는 전역 객체를 가리키고 있다는 것을 확인 할수 있다.



### 함수 호출 패턴
- 함수가 객체의 속성이 아닌 경우 함수로서 호출 된다.
- 함수 내부의 this는 함수를 호출한 방법에 의해 좌우된다.
- 함수 호출 패턴으로 함수를 호출 하게 되면 this는 전역 객체에 바인딩 된다.

예제2
~~~javascript
function func(){
  return this;
}

func() === window;
//true
~~~
예제2에서 `func` 함수의 호출은 `window.func()`와 같다. `func` 함수를 호출한 window를 this가 가리킨다.

예제3
~~~
function obj(value) {
  this.value = value;
}

Obj.prototype.double = function() {
  var _this = this;
  function helper(){
    return _this.value + _this.value;
  }
  helper();
}
~~~
예제3은 함수 호출 패턴 사용시 주의 할 점이다. 위 예제에서 `Obj` 객체의 double 메소드에서 `Obj`가 바인딩 된 this를 사용 하기 위해서는 `var _this = this;`을 꼭 작성해 주어야 한다. 그 이유는 객체 내부에 속성이 아닌 함수를 호출 할때에는 함수 호출 패턴을 사용하는데 `helper`함수의 안에서 this를 사용 했다면 무조건 전역 객체를 가리켜 원하는 동작을 하지 않았을 것이다.  


### 메소드 호출 패턴
- 메소드 : 함수가 객체 내부의 property.
- 어느 한 객체의 메소드로 호출 되면 this는 호출한 객체를 가리킨다.
- 메소드를 호출하면 this는 메소드를 호출 하고 있는 객체에 바인딩이 된다.
- 메소드는 자신을 포함하고 있는 객체의 속성들에 접근을 위해 this를 사용할 수 있다. 
- this를 통해 객체의 값을 read / update 가능하다.

예제4
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

/*
결과
dhkim
29
false
*/
~~~
위의 예제는 우선 `Person` 객체를 정의후 `person1`인스턴스를 생성한다. `person1`인스턴스를 통해 name과 age를 참조하면 참조 한 인스턴스인 `person1`을 this가 가리킨다. 그리고 해당 `Person` 객체의 prototype에 `isYoung` 메소드를 정의 하고 `person1`인스턴스를 통해 호출 하게 되면 `this.age`의 this는 `person1`을 가리켜 false 값을 제공한다. 이 예제와 같이 객체의 메소드로서 할당 될때는 해당 메소드를 호출한 객체를 this가 가리킨다. 

> **주의** this 동작은 함수가 정의된 방법이나 위치에 전혀 영향을 받지 않는다.<br>
> 예제3 Person 객체의 `isYoung`메소드는 객체 인스턴스 후에 정의 되었어도 제대로 동작 하는 것을 볼 수 있다. 여기서 중요한 것은 **`isYoung` 메소드가 person에 의해 호출 된 사실이 중요한 것이다.**


### 생성자 호출 패턴
- 자바스크립트의 생성자 함수는 객체를 생성하는 역할을 한다. 
- 일반적인 함수에 `new` 연사자를 붙여 호출 하면 해당 함수는 생성자 함수로 동작 한다. 
- 일반 함수에 new 연산자를 붙이는 것이 아니라 함수의 맨 앞글자가 대문자인 함수를 생성자 함수로 암묵적으로 정의 한다.
- new와 함께 생성자 함수를 호출하는 경우, 생성자 함수 내부의 this는 생성자 함수에 의해 생성된 인스턴스를 가리킨다. 

#### 생성자 함수 동작 방식
1. 빈 객체 생성({})과 this 바인딩
2. this를 사용한 property를 생성한다. 
3. 생성된 instance를 반환

예제4
~~~Javascript
var Person = function(name) {
  this.name = name;
}

var me = Person('Lee');

console.log(me);
console.log(window.name);

/*
결과
undefined
Lee
*/
~~~
생성자 함수에 new키워드를 붙이지 않아도 정상적인 작동을 한다. 하지만 new키워드를 붙이지 않으면 `Person` 함수에서 암묵적으로 return해주던 this도 리턴 해주지 않기 때문에 기본적인 return 인 undefined가 me에 할당 된다. 그리고  `Person` 함수 내부에 정의 된 `this.name = name;` 구문은 실행시 함수 호출 패턴에 의해 this는 window를 가리키게 되고 `console.log(window.name);` 구문에 의해 Lee가 출력된다.

### apply 호출 패턴(Apply Invocation Pattern)
- this는 함수 호출시 바인딩 될 객체가 정해진다. 하지만 apply나 call을 사용하게 되면 원하는 객체를 this에 바인딩하고 여러가지 인자도 제공 한다.
- 자세한 설명은 https://github.com/DongHyunKims/javascript-notes/tree/master/notes/bind%2Ccall%2Capply 에서 확인 바람.

### DOM 이벤트 핸들러
- 함수가 이벤트 핸들러로 사용 하면 this는 이벤트를 발생한 객체롤 바인딩 된다.

예제5
~~~Javascript
var element = document.querySelector(".successBtn");
element.addEventListener("click",function(event){
  console.log(this === event.currentTarget());
});
/*
결과
true
*/
~~~
위 예제에서 함수를 이벤트 핸들러용 익명 함수 안에서 this를 사용하면 현재 이벤트가 발생 하고 있는 element 객체를 바인딩 한다.



### Default Binding

this는 global을 바라보고 있다. 하지만, strict mode를 적용하면 this는 undefined가 된다.




### 참고
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this
- http://resoneit.blogspot.kr/2014/01/this.html
- http://poiemaweb.com/js-this
