## bind/call/apply

#### 1. call
  - 함수를 호출하는 방법 중 하나.
  - call() 메소드는 주어진 this 값 및 각각에게 제공된 인수를 갖는 함수를 바로 호출.
  - call의 첫번째 인자는 지정할 context이며, 이 후의 인자로는 그 함수에 전덜해줄 arguments를 하나씩 제공한다.
  - `fun.call(thisArg[, arg1[, arg2[, ...]]])`

  ~~~
  function greet() {
    var reply = [this.person, 'Is An Awesome', this.role].join(' ');
    console.log(reply);
  }

  var i = {
    person: 'Douglas Crockford', role: 'Javascript Developer'
  };

  greet.call(i);
  ~~~
  - 위 예제에서 greet 함수를 호출하 였을때 this는 i 객체. call 메소드를 통해 context를 "i" 객체로 정해 주었기 때문에 호출 시 해당 context가 정해지는 this는 i로 바뀌게 되고 console.log로 제공 되는 결과는 "Douglas Crockford Is An Awesome Javascript Developer" 이다.

#### 2. apply
  - apply() 메소드는 주어진 this값 및 배열로 제공한 arguments로 함수를 바로 호출.
  - 호출할 함수의 인자로 유사배열객체, 특히 함수가 호출될 때 생성된 arguments객체를 전달.
  - appy의 첫번째 인자는 지정할 context이며, 이 후의 인자로는 그 함수에 전덜해줄 arguments를 유사배열 객체로 제공.
  - `fun.apply(thisArg, [argsArray])`

  ~~~
  /* min/max number in an array */
  var numbers = [5, 6, 2, 3, 7];

  /* using Math.min/Math.max apply */
  var max = Math.max.apply(null, numbers);

  console.log("max" + max);                   
  ~~~
  - 위 예제와 같이 apply를 내장 함수와 같이 사용한다. 위 예제 에서 apply의 첫번째 인자로 null을 정해 주어 context는 window 객체가 된다. window 객체의 max메소드를 numbers라는 배열을 두번째 인자로 주게 되면 해당 배열의 값을 순서대로 호출한 메소드의 인자로 보내 실행한다. 위의 예제에서 console.log로 제공되는 결과는 "7" 이다.

#### 3. bind
  -  bind() 메소드는 호출될 때 그 this 키워드를 제공된 값으로 설정하고 새로운 함수가 나중에 호출될 때 첫번째 인자로 주었던 인자로 context를 설정하고 두번째 인자 이후의 값을 arguments로 주어 함수를 실행.
  - apply, call과 차이점은 함수를 바로 실행 시키는 것이 아니라 나중에 호출 될때의 context와 필요한 인자를 정함.
  - EventListener의 콜백함수의 context를 정할 때 많이 사용.
  - `fun.bind(thisArg[, arg1[, arg2[, ...]]])`

  ~~~
    this.x = 9;
    var module = {
      x: 81,
      getX: function() { return this.x; }
    };

    module.getX(); // 81

    var retrieveX = module.getX;
    retrieveX();

    var boundGetX = retrieveX.bind(module);
    boundGetX(); // 81
  ~~~
  - 위 예제는 this.x를 통해 전역 x에 9를 저장하고 module 객체의 x에 81을 저장한다. `module.getX();`의 getX()메소드의 this는 module이기 때문에 "81"이 결과로 출력된다. `var retrieveX = module.getX;` 구문에서는 retrieveX에는 getX 함수가 할당 되고 retrieveX 함수를 실행 시키면 this는 전역이 되고 "9"가 결과로 나온다. 하지만 `var boundGetX = retrieveX.bind(module);` 구문에서 bind 함수를 사용하여 context를 module 객체로 정한후 boundGetX함수를 실행시키면 bind 함수에 의해 this는 `module`이 되고 결과는 "81"이 제공된다.
~~~
  // The .bind method from Prototype.js
  Function.prototype.bind = function(){
  var fn = this, args = Array.prototype.slice.call(arguments), object = args.shift();
    return function(){
      return fn.apply(object,
      args.concat(Array.prototype.slice.call(arguments)));
    };
  };
~~~
  - 위 예제는 bind를 임의로 구현한 것이다. bind를 실행하면 fn 변수에는 현재 bind를 호출한 함수의 context를 저장 하고 args 변수에는 유사 배열 객체인 arguments을 배열로 복사한 값을 저장한다. object에는 args의 첫번째 인자로 들어온 값을 shift메소드를 통해 저장한다. 그 후 함수를 리턴 하게 되는데 해당 함수가 실행 되면 fn에 저장한 함수를 apply 메소드를 통해 바로 실행 시킨다.  object 변수의 값을 context로 정하고 args 배열과 리턴된 함수의 arguments 배열을 concat 메소드를 통해 하나의 배열로 만들어 두번쨰 전달인자로 제공하여 해당 함수를 바로 실행 시킨다.


  #### 출처
  - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference
  - http://ejohn.org/apps/learn/#2
  
