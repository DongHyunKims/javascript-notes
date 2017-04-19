## javascript checklist 1~4

### 1. 자바스크립트 scope를 var키워드르를 기준으로 설명할수 있다.
### 2. closure 는 언제 형성되는지? 경험한 코드가 있으면 코드로 보여주기.
### 3. const는 언제 사용해야 하는지?
**특징**

- const는 let과 동일하게  block scope을 가진다.
- const는 재할당이 되지 않는 속성 때문에 선언후 바로 초기화 하여야 한다.
~~~javascript
const NUM = 123;
NUM = 456; 
// TypeError: Assignment to constant variable.
~~~

- const는 유지보수와 가독성을 위해 사용한다.(다른 언어의 상수 선언 처럼 사용. 하지만 상수와 같이 절대 불변 하지는 않음.)
~~~javascript
const MAX = 10;
if (x > MAX) {
}
~~~

- 객체의 프로퍼티는 보호되지 않는다. (재할당은 불가능하지만 할당된 객체의 내용은 변경 가능.)

~~~javascript
const PERSON = {
  name: 'kim',
  address: {
    city: 'Seoul'
  }
};

user.name = 'Lee';

console.log(user); 
// { name: 'Lee', address: { city: 'Seoul' } }

Object.freeze(user);

user.name = 'Kims';

console.log(user); 
// { name: 'Lee', address: { city: 'Seoul' } }
~~~
위의 예제에서와 같이 PERSON 이라는 변수에 재할당은 불가능하지만 PERSON의 property 수정은 가능하다. 객체의 property까지 보호하기 위해서는 `Object.freeze()` 메소드를 사용 해야한다. `Object.freeze()`메소드를 사용하면 객체의 프로퍼티까지 보호 가능하다.

~~~javascript
user.address.city = 'Newyork';

console.log(user); 
// { name: 'Lee', address: { city: 'Newyork' } }
~~~
하지만 객체안의 객체의 property의 변경은 허용 된다. 이런 경우에는 내부에 있는 객페도 `Object.freeze()`메소드를 사용하여 변경을 막아야 한다. 

~~~javascript
function deepFreeze(obj) {
  const props = Object.getOwnPropertyNames(obj);
  props.forEach((name) => {
    const prop = obj[name];
    if(typeof prop === 'object' && prop !== null) {
      deepFreeze(prop);
    }
  });
  return Object.freeze(obj);
}

deepFreeze(user);
user.address.city = "Busan";

console.log(user); 
// { name: 'Lee', address: { city: 'Newyork' } }
~~~
`deepFreeze` 함수를 이용해서 객체의 property 전체를 변경 못하게 할수 있다. 

**언제 사용하는가?**
- ES6를 사용시 `var` 보다는 `let`,`const` 사용을 지향.
- 변경이 발생하지 않는(재할당이 필요없는) primitive형 변수와 객체형 변수에 const를 사용.
- const는 유지보수와 가독성을 위해 사용.
- const는 상수(변하지 않는 값)를 위해 사용.




### 4. mvc방식으로 개발한 사례가 있다면 본인이 느끼는 장단점을 설명하기.