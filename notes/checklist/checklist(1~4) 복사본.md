## javascript checklist02

### 6. 비동기 콜백함수와 스택간의 관계를 어떻게 설명할 수 있는지?

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