## Async/Callback

#### 1. Async 와 Sync
1. Sync 란?
- 실행 순서가 절차 적이다.
- 코드의 작성 순데대로 실행된다.

2. Async 란?
- Async는 전 명령의 수행이 끝나지 않아도 다음 명령을 실행한다는 의미이다.

3. 예제 : 작성한 이메일을 1000명에게 보낸다.
> <img src="../../images/Async.png" width="300">
> <img src="../../images/sync.png" width="300">

- 위 예제 와 같이 Async는 이메일 발송 작업 예약 후 백그라운드에서 진행 하고 이메일 작성을 먼저 완료 할수 있다. 이메일 발송은 백그라운드에서 진행하며 사용자는 다른 작업 수행이 가능하다. 하지만  sync는 절차적으로 명령이 수행 되며 1000명에게 이메일 발송이 끝나야 이메일 작성이 완료 되며 이메일 발송을 수행하는 동안은 다른 작업을 수행 할 수 없다.

4. javascript에서 왜 Async 가 중요한가?
- javascript는 다른 언어와 다르게 단일 스레드로 동작한다. 단일 스레드는 하나의 스레드가 모든 명령을 처리 해야 하기 때문에 동기적으로 처리한다면 연산이 많은 로직을 처리할때는 그 연산이 끝날때 까지 계속 기다려야 하는 이슈가 있다. 그렇기 때문에 비동기적 처리를 통해 시간이 많이 걸리는 연산들을 백그라운드에서 처리하고 다른 작업을 수행 할수 있도록 하기위해 Async 개념은 꼭 필요하다.

5. 코드 예제
- 코드
~~~javascript
for (var i = 0; i < 10; i++) {
	setTimeout(function() {
		console.log(i);
	}, 10);
}
console.log('done');
~~~
- 결과 
~~~
done
10
10
10...
~~~
-  우선 for 문을 수행 후 맨 아래에 있는 console.log를 먼저 수행 한다. 그 후 queue에 들어있던 setTimeout의 콜백 함수를 실행 시키기 때문에 위의 결과가 제공 되었다.


#### 2. Callback
1. Callback 이란?
- JavaScript에서는 함수도 객체이기 때문에 파라미터로 넘길 수 있고, 넘겨받은 함수를 언제 실행할지 결정할 수 있다. 모든 명령의 실행을 마친 후에 넘겨받은 함수객체를 실행시키는 것도 가능한데 이것이 바로 Callback 함수 이다.
- parameter로 함수를 전달하는 의미도 있다.

2.  코드 예제
-  코드 
~~~javascript
document.getElementById("myBtn").addEventListener("click", function(){
    document.getElementById("demo").innerHTML = "Hello World";
});
~~~
- 위의 예제는 우리가 많이 사용하는 이벤트 리스너이다. 버튼을 클릭시 해당 이벤트가 발생하면 지정 해둔 콜백함수가 실행 되도록 한다. 이와 같이 콜백함수를 예약 해놓고 이벤트가 클릭 되면 해당 콜백함수를 실행 시키도록 비동기 처리를 하였다.

3. Callback 함수 사용시 Call 과 Apply를 통한 this 보호
- callback으로 전달된 함수안에서 this를 사용하면 this의 특성(실행될때 context가 정해짐)으로 인해 복잡해질 가능 성이 크다. 이러한 단점을 해결하기위해 call, apply, bind 함수들을 통해 context를 지정 해준다.

~~~javascript
function getUserInput(firstName, lastName, callback, callbackObj) { 
	callback.apply (callbackObj, [firstName, lastName]); 
}
~~~
- 위의 예제와 같이 callback함수에서 사용되는 context인  callbackObj를 함께 전달하여 apply 함수를 통해 this의 context를 보호 할수 있다.




#### 출처
- http://yubylab.tistory.com/entry/자바스크립트의-콜백함수-이해하기
- https://hyunseob.github.io/2015/08/09/async-javascript/


