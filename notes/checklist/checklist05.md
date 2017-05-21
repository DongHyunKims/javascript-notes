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

