## 자바스크립트 정규표현식 1

요즘 회사에서 크롤링 작업을 하고 있는데 정규표현식을 굉장히 많이 사용하고 있다. 아직 정규표현식이 서툴러 제대로 사용을 못하고 있어 이전에 배운 정규표현식 내용을 정리한다.

**정규표현식(regular expression)이란?**
특정한 규칙을 가진 문자열의 집합을 표현하는데 사용한다. 일반적으로 문자열의 검색과 치환을 위해 사용된다.
쉽게 말하자면 일정한 규칙으로 문자열의 내용을 검색하거나 변경할때 사용한다고 볼수 있다.

- 장점: 특정 조건의 문자를 간단히 추출 할수 있다.
- 단점: 정규식 알지 못한다면 이해하기 힘들다.

정규표현식에서 사용하는 기호를 Meta 문자라고 하며 표현식 내부에서 특정한 의미를 가진다. 정규식은 다섯개의 **Flag**를 설정 할 수 있으며, 전역 검색 또는 대소문자 구분 없는 검색이 가능하다.

- Meta 문자나 Flag에 대한 자세한 내용은 https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D  참조

**실전 정규표현식**

1.  커서와 글로벌 플래그

~~~ javascript
// rxFoo라는 정규표현식 객체를 생성한다.
const rxFoo = /foo/g;
const text1 = 'foo foo';

// 커서의 이동을 확인

// foo| foo
console.log(rxFoo.test(text1));
console.log(rxFoo.lastIndex);

// foo foo|
console.log(rxFoo.test(text1));
console.log(rxFoo.lastIndex);

console.log(rxFoo.test(text1));
console.log(rxFoo.lastIndex);
~~~
https://jsbin.com/pibapa/edit?js,console

글로벌 플레그(g)를 사용한 정규표현식 객체의 test 메소드를 사용하면, 탐색 커서의 위치가 변경되는 것을 확인 할 수 있다. 커서의 위치가 변경되기 때문에 3번째 탐색에서는 false가 제공되는 것을 확인 할수 있다. 반복적인 탐색을 위해서는 lastIndex를 0으로 셋팅 해야한다.

~~~ javascript
rxFoo.lastIndex = 0
~~~

**문제1**

~~~ javascript
const rxFoo = /foo/g;
const test1 = 'foo test test foo good';
let cnt1 = 0;
while (rxFoo.test(test1)) {
	cnt1++;
}
console.log(cnt1) // ??
console.log(rxFoo.lastIndex) // ??

let cnt2 = 0;
while (/foo/g.test(test1)) {
	cnt2++;
}
console.log(cnt2); // ?
~~~

2. 캡쳐링 그룹

~~~ javascript
// 전화번호 정규표현식
const rxPhone = /(\d{3}-\d{3,4}-\d{4})/;

// test string
const str = `
  Name: alex
  Phone: 010-1234-5678
  Address: Pangyo
`;

let phone = '';

if (rxPhone.test(str)) {
	// 즉, 정규표현식의 /() -> $1 () -> $2/ 매칭
	phone = RegExp.$1;
}

console.log(phone);
~~~
https://jsbin.com/pizihos/edit?js,console

정규표현식에서  캡쳐링그룹 **()** 을 사용하면 RegExp 상수에 $(n)으로 접근하여 캡쳐링 그룹에 탐색된 순서대로 string 추출이 가능하다.

**문제2**

밑 2개의  string을  output의 형태로 만드시오.

~~~ javascript
const str = `
  Id: 1 / Name: Foo
  Id: 20 / Name: Bar
  Id: 300 / Name: Alex
  Id: 4000 / Name: Key
`;

const str2 = `
Id: 1 / Name: Foo
  id:         20   / name   : Bar
    ID: 300 /  NAME   : Alex
 id   : 4000 / name  :   Key
`;

/*
	output

	{
		1: Foo,
		20: Bar,
		300: Alex,
		4000: Key,
	}
*/
~~~

https://jsbin.com/cecudav/edit?js,console

3. 멀티라인 플래그 활용
- \n, \r  이 포함된 문자열에서  원하는 문장을 추출할때 사용하는 플래그
- ^, $은 본래 문장에 시작과 끝을 나타내는데 m플래그를 사용하면 시작은 \n, \r 뒤, 끝은 \n, \r 뒤를 의미한다.

**문제3**

~~~ javascript
const str = `
  2017.10.20,7423
  #2017.10.13,5234
  2017.09.29,1534
  #2017.09.22,4353
  2017.09.15,5123
  2017.09.08,1523
`;

// output
/*
	{
		7423: 2017.10.20
		5234: 2017.10.13
		153: 42017.09.29
		...
	}
*/
~~~
https://jsbin.com/pekatek/edit?js,console
