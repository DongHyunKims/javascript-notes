## 자바스크립트로 크롤링 하기 1

**크롤링(crawling) 이란?**

Web Scraping이라고도 하며 웹사이트의 정보를 긁어모아서 Data Base화 시키는 행위를 의미 한다. 이러한 방식을 통해 구글, 네이버 등 여러 사이트의 정보를 수집한다고 생각하면 된다.

이번에 회사 프로젝트를 통해 웹 크롤링을 하게 되었다. 아직 프로젝트가 완성되지는 않았지만 웹 크롤링을 하면서 어려웠던 점을 정리하려고 한다.

**사용 언어, 모듈**

언어는 자바스크립트, 모듈는 cheerio, request 라이브러리를 사용하여 웹 크롤링을 진행 하였다.

**cheerio**

- 마크업 구문을 parsing하고, 데이터 구조를 탐색/조작을 위해 사용
- cheerio는 jQuery의 문법을 사용법과 유사하여 사용하기 쉬움 (jQuery selector 사용)
- 단순하고 일관된 DOM 모델로 작동되므로 속도가 빠름
- HTML을 파싱하여 필요한 정보만 가져올수 있음

**request**

- 간단한 HTTP 클라이언트
- 웹 사이트의 HTML 문서를 가져오기 위해 사용

예제
~~~ javascript
const cheerio = require('cheerio');
const request = require('request');

function crawling() {
	const options = {
		url: ...
	}

	// request 모듈을 사용하여 원하는 url로 call 전송
	request(options, function(error, response, body) {
		// 해당 콜에 대한 응답을 받아 cheerio를 통해 파싱, jQuery처럼 사용하기위해 $변수에 할당
		const $ = cheerio.load(body);
		const $trs = $.find('.table1 tbody tr');
		const data = {};

		for (let i = 0; i < $trs.length; i++) {
			const $tds = $trs[i].find('td');

			for (let j = 0; j < $tds.length; j++) {
				if (j === 1) {
					data.title = $tds[j].text();
				}

				if (j === 2) {
					data.content = $tds[j].text();
				}
			}
		}
	});
}
~~~

1. request 모듈을 사용하여 해당 url로 call을 전송
2. 해당 call에 대한 응답이 정상적으로 올 경우 cheerio.load를 통해 body를 파싱
3. jQuery selector를 사용하여 HTML element에 접근
4. 원하는 정보 추출

**어려웠던점**
사실 프로젝트 수행시 다양한 변수가 존재해서 위 예제처럼 쉽게 데이터를 추출할 수 없었다.

- 쿠키정보를 통해서 사이트 정보가 변경되는 경우
	*request 헤더를 분석해 봐도 query string, form 데이터를 통해 정보를 보내지 않고, session 값을 통해 데이터값을 바꾸는 페이지가 있어 원하는 정보를 파싱하는데 한참 걸렸었다...*

- HTML element가 뒤죽박죽인 경우
	~~~ html
	<ul>
			<-- 여는 <li> 태그가 존재 하지 않음 -->
			<span>
				test1
			</span>
		</li>
	</ul>
	~~~

- 페이지의 인코딩이 제대로 되어있지 않아 문자가 깨져서 나오는 경우
	*iconv 모듈을 사용하여 해결하였는데 iconv 모듈 사용법도 따로 포스팅 하겠다.*

- 	데이터가 제대로 제공되지 않는 경우 가공이 어려움
	*예를 들어 여러개의 사이트에서 일관된 정보를 얻고 싶은데 각 사이트 마다 작성 방법이 달라 원하는 정보로  파싱하는것이 굉장히 까다로웠다. 이때 필요한 것이 정규 표현식이다. 정규표현식을 잘 알고 있다면 원하는 정보를 얻는데 아주 수월해 질 것이다. 정규표현식 에  대한 내용도 포스팅 하도록 하겠다.*


**해결방법**
- 쿠키정보를 통해서 사이트 정보가 변경되는 경우 header에 해당 쿠키 정보를 추가해 콜을 보내면 해결된다.
~~~ javascript
function test() {
	const options = {
		url: ...,
		header: {
			Cookie: `JSESSIONID=...; popup=...; ...`
		}
	}
	request(options, function(error, response, body) {
		...
	});
}
~~~


- HTML element가 뒤죽박죽인 경우 조건에 따라 다르게 파싱하는 방법을 사용했는데 javascript 모듈중 깨진 DOM을 정상적으로 변환해주는 모듈이 있다고 하여 사용해 볼 생각이다.

**결론**

크롤링 작업시 기본적으로 필요하다고 생각하는 4가지가 있다.

1. HTTP에 대한 이해 (requst, response)
2. 크롬 개발자 도구 사용 (Elements, Network)
3. 정규 표현식
4. DOM에 대한 이해 (DOM 구조, 패턴 파악)

물론 이외에도 더 많은 지식이 필요하겠지만 위 4가지에 대한 기본적 이해가 있다면 어렵지 않게 크롤링 작업을 할 수 있을 것이다.
크롤링은 다양한 방법으로 가능하다. 스크립트를 실행시키거나 로그인후 데이터를 크롤링 해야 하는경우 js DOM, phantom js등 다른 모듈을 고려해 보는 것도 좋은 방법이다.

**참조**

requset: https://github.com/request/request
cheerio: https://github.com/cheeriojs/cheerio









