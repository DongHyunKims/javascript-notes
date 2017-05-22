## javascript checklist05

### 19. jQuery를 사용하는 것의 장점과 단점은 무엇이라고 생각하는지.

**설명**

1. JQuery 란 ? 

- 엘리먼트를 선택하는 강력한 방법과 선택된 엘리먼트들을 효율적으로 제어할 수 있는 다양한 수단을 제공하는 자바스크립트 라이브러리
- The Write Less, Do More : 적게 작성하고 많은 것을 할수 있다. 

2. JQuery의 장점 
- '멀티 브라우저 지원' : 어느 브라우저에서나 동일 하게 작동한다.
- element 의 속성이나, 이벤트 부여를 쉽게 한다.
- 메소드 chaining을 지원.

3. JQuery의 단점
- 성능 문제 : JQuery의 크기는 32k 나 된다. 압축을 해도 이정도의 크기이면 어느 웬만한 사이트 javascript파일 전체를 합쳐놓은 듯한 크기이다. 어느 일부의 기능만 사용하기 위해 이 정도 크기의 파일을 삽입 한다는 것은  너무나도 비 효율 적이다.
- JQuery 역시 vallia js 이다. 더 좋은 성능을 가지고 있을 수록, 많은 조작이 필요 하기 때문에 일반 javascript 사용 보다 느리다.
- 라이브러리 이므로 새로 배워야 한다.


### 30. javascript 코드를 여러개 html에 추가하는 것과, 하나로 머지하여 배포하는 것의 장단점은 무엇인가? 





**참조**
- http://d2.naver.com/helloworld/0239818

### 8. 배열에서 제공하는 메서드 중에 filter와 map의 활용경험은? 

1. map 
- array 의 메소드로서 본 배열의 값을 조작하여 같은 길이의 새로은 배열을 만드는 메소드
- map은 callback함수를 각각의 요소에 대해 한번씩 순서대로 불러 그 함수의 리턴값(결과값)으로 새로운 배열을 만든다.

~~~
['1', '2', '3'].map(Number); // [1, 2, 3]
~~~


2. filter
- array 의 메소드로서 본 배열의 원소들 중 조건의 맞는 값으로 새로운 배열을 만드는 메소드.
- filter()는 배열 내 각 요소에 대해 한 번 제공된 callback 함수를 호출해, callback이 true로 강제하는 값을 반환하는 모든 값이 있는 새로운 배열을 생성.

~~~
function isBigEnough(value) {
  return value >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
~~~

저는 React 사용시 반복되는 component를 생성시 map 함수를 사용했습니다. 그리고 playList 삭제시 원하는 id의 video를 걸러낼때 filter 메소드를 사용 했습니다.

### 16. setTimeout을 이용해 재귀호출을 하는 경우 어떤 장점이 있는가? 

**참조**
- http://www.bsidesoft.com/?p=399

### 58. 네임스페이스 패턴의 장점은? 

1. Namespace Pattern이란?

네임스페이스(namespace)는 구분이 가능하도록 정해놓은 범위나 영역을 한다. 즉, 말 그대로 이름 공간을 선언하여 다른 공간과 구분하도록 한다.

대개 애플리케이션이나 라이브러리를 위해  **전역 유효 범위에 수많은 함수, 객체, 변수들로 어지럽히지 않도록 하기 위해 전역 객체를 하나 만들고(단 하나만 만드는 것이 이상적이다) 모든 기능을 이 객체에 추가하는 패턴**을 네임스페이스 패턴이라고 한다.

**예제1**
~~~Javascript
// 수정 전 : 전역 변수 5개 
// 경고 : 안티 패턴 
// 생성자 함수 2개 
function Parent() {} 
function Child() {} 
// 변수 1개 var some_var = 1; 
// 객체 2개 var module1 = {}; 
module.data = { a : 1, b : 2 }; 
var module2 = {}
~~~

**예제2**
~~~Javascript
// 수정 후 : 전역 변수 1개 
// 전역 객체 
var MYAPP = {}; 
// 생성자 
MYAPP.Parent = function() {}; 
MYAPP.Child = function() {}; 
// 변수 
MYAPP.some_var = 1; 
//	객체 컨테이너 
MYAPP.modules = {}; 
// 객체들을 컨테이너 안에 추가한다. 
MYAPP.modules.module1 = {}; 
MYAPP.modules.module1.data = { a : 1, b : 2 }; 
MYAPP.modules.module2 = {};
~~~

흔히 코드를 읽는 사람 눈에 띄도록 전역 객체 이름은 모두 대문자로 쓰는 명명 규칙을 사용하기도 한다.(이 규칙은 상수를 쓸 때도 사용된다는 점에 주의하라.)

**이 패턴은 코드에 네임스페이스를 지정해주며, 코드 내의 이름 충돌 뿐 아니라 이 코드와 같은 페이지에 존재하는 자바스크립트 라이브러리나 위젯 등 서드 파티 코드와의 이름 충돌도 방지해 줄 수 있다.**

다양한 작업에 응용할 수 있으며, 매우 권장하는 패턴이다. 

**단점**

- 모든 변수와 함수에 접두어를 붙여야 하기 때문에 전체적으로 코드량이 약간 더 많아지고 따라서 다운로드해야 하는 파일 크기도 늘어난다.
- 전역 인스턴스가 단 하나뿐이기 때문에 코드의 어느 한 부분이 수정되어도 전역 인스턴스를 수정하게 된다. 즉 나머지 기능들도 갱신된 상태를 물려 받는다.
- 이름이 중첩되고 길어지므로 프로퍼티를 판별하기 위한 검색 작업도 길고 느려진다. 

흔히 코드를 읽는 사람 눈에 띄도록 전역 객체 이름은 모두 대문자로 쓰는 명명 규칙을 사용하기도 합니다.(이 규칙은 상수를 쓸 때도 사용된다는 점에 주의하라.)


**예제3**
~~~Javascript
var MYAPP = MYAPP || {}; 
MYAPP.namespace = function (ns_string) { 
var parts = ns_string.split('.'),
parent = MYAPP, 
i; 
// 처음에 중복되는 전역 객체명은 제거한다. 
if(parts[0] === MYAPP) { 
parts = parts.slice(1);
} 
for (i = 0; i < parts.length; i += 1) { // 프로퍼티가 존재하지 않는다면 생성한다. 	if(typeof parent[parts[i]] === 'undefined') { parent[parts[i]] = {}; } 	  parent = parent[parts[i]]; 
} 
return parent; 
};
~~~
다음과 같은 방식은 해당 네임스페이스가 존재하면 덮어쓰지 않기 때문에 기존의 코드를 망가뜨리지 않는다.

**주의**
namespace는 자바스크립트에서 사용하고자, 일반적인 사용을 금지한 예약어에 포함되어 있다.
따라서 이 단어 그대로를 프로퍼티명으로 쓰는 것은 권하지 않는다.



