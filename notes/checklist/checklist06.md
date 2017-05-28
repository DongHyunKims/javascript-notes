## javascript checklist06

### 13. 좋은 코드를 만들기 위해서 노력한 경험들은 무엇인지? 

### 14. 전역변수를 없앨 수 있는 방법들은 무엇인가? 

1. namespace 패턴 사용하기.
2. 모듈 패턴 사용하기. - 28번 참조.

**참조**
- https://www.w3schools.com/js/js_best_practices.asp

### 20. 정규표현식으로 email을 체크해보기.

### 22. 서비스 성능개선을 했던 사례를 말해보자.

### 25. bind 메서드의 내부 코드는 어떻게 됐을까?

### 28. 모듈패턴은 어떻게 구현할수 있는지? 모듈패턴의 어떤 장점이 있는지?
**설명**

- 모듈 패턴은 자바스크립트의 즉시 실행 함수, 함수 scope, closure를 이용한다. 
- 내부의 상태나 구현 내용을 숨기고 인터페이스만 제공하여 내부 접근을 제한 할수 있다.
- 모듈 패턴을 사용하면 전역변수를 최소한으로 사용할 수 있기때문에 자비스크립트의 약점을 보완 할수 있다.

**종류**
1. 기본 모듈 패턴
~~~Javascript
var PrintModule = (function(){
         var message = "Hello World",
             log = function(){
                 console.log("log : ", message);
             };
         //공개할 API
         return {
             log : log
         }
})();
~~~
- 모듈 패턴은 즉시 실행 함수로 모듈이 될 객체를 반환한다. 즉시 실행 함수를 사용하면 외부에서 `message` 변수에 접근 하지 못하기 때문에 값을 변경 할 수 없다.

2. 생성자 모듈 패턴
~~~Javascript
var PrintModule = (function(){
         var message = "";
         var PrintModule = function(msg){
              message = msg;
          }
          PrintModule.prototype.log = function(){
              console.log("log : ", message);
          };
          return PrintModule;
})();
~~~
- 기본 모듈 패턴과 거의 비슷하며, 생성자 함수를 반환하는 점이 다르다.

3. 인자를 넘기는 모듈 패턴
~~~Javascript
 window.message = "Global Message";
      var MyApp = MyApp || {};
      MyApp.PrintModule = (function(app, global){
          var log = function(){
              console.log("log : ", global.message);
          };
          return {
              log : log
          }
})(MyApp, window);
~~~

- 즉시 실행 함수를 실행시키면서 인자를 전달하여, 전역변수, 전역 객체를 참조하기 위해 전달한다. 전역변수를 전달하여 함수 내의 지역변수로 사용 할수 있기 때문에 탐색이 빨라지는 장점이 있다. 

**참조**

- https://yuiblog.com/blog/2007/06/12/module-pattern/
- http://hyeonmi.github.io/post/module-pattern/
### 53. REST API는 무엇인가? 
- REST : REPresentational State Transfer
- 어떻게 전송을 할것인가? 
- 데이터를 주고 받을때 나아진 방법을 이야기 한다.


### 34. DOM Tree란 무엇인가? 