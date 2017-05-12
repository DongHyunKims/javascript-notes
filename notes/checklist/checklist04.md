## javascript checklist04
### 44. 팀프로젝트에서 제일 중요한 부분은 무엇이라고 생각하는가? 

팀프로젝트에서 가장 중요한 부분은 팀 룰, 소통 이라고 생각한다. 팀 프로젝트라는 것은 말 그대로 혼자가 아닌 팀원 들이 함께 하나의 서비스를 만들어 나가는 것이다. 만약 팀 룰이 없이 프로젝트를 진행하게 되면 팀원 각각의 스타일과 목표치가 다르기 때문에 팀프로젝트를 진행하기 어려워 지며, 나중에는 불협화음일 일어나게 된다. 그렇기 때문에 팀원 끼리  팀 룰, 컨벤션 등을 함께 만들고 그 룰을 지킨다면 팀원모두 일관성 있는 프로그래밍이 가능해 질  것이다. 그리고 항상 팀원들의 상태, 목표, 계획등을 소통을 통해 공유 하면 프로젝트를 계획에 차질이 생기더라도 빠르게 대처 할 수 있을 것이다.

### 45.  커뮤니케이션을 잘하기 위해서 어떤 방법을 해보았는가? 

팀원들과 커뮤니케이션을 잘 하기 위해 코드리뷰, 데일리 회고, 데일리 스크럼, 페어프로그래밍 등을 해보았다. 특히 데일리 회고를 통해 프로젝트가 어느 정도 까지 진행 되었으며, 팀원의 어려운 점 등을 공유 할수 있어 팀프로젝트 진행에 많은 도움이 되고있다.

### 46. 커밋로그가 가진 의미는 어떤것일까? 어떤 규칙이 좋다고 생각하는지?
- 커밋 : 마무리된 작업에 작업이력을 기록해서 저장소로 보내는 행위

커밋 로그는 작업도중 어떠한 작업이 완성되면 해당 작업이력을 작성한다. 팀원들은 커밋 로그를 통해 프로젝트가 어떻게/ 어디가 수정되었는지 알고 이후의 작업을 어떻게 진행 하는지에 대한 척도가 된다. 그러므로 커밋 로그는 팀원 모두 함께 룰을 정하는 것이 좋으며, 간결하고 알기 쉽게 작성해야한다. 가독성이 좋고 명시적이며, 규칙적인 커밋로그가 좋다. 

**예**
커밋 로그 제목 : [Add] App.js - [] : 메이저, () : 마이너, `.`은 사용하지 않음 , 50자 이내.
커밋 로그 본문 : 1. App.js에서, 데이터 조작 성공. - 개조식으로 작성, 제목과 2칸 띄움 등..

### 52. merge 과정에서 생기는 충돌을 해결하는 과정을 설명하세요. 

팀원들이 자신이 작업한 branch에서  하나의 파일을 동시에 수정 하고 하나의 branch에 merge 할 경우 conflict가 발생한다. 

~~~bash
$ git merge issue3
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~

함께 수정한 파일 내용은 다음과 같이 변경되어있다.
~~~text
원숭이도 이해할 수 있는 Git 명령어
add: 변경 사항을 만들어서 인덱스에 등록해보기
<<<<<<< HEAD
commit: 인덱스의 상태를 기록하기
=======
pull: 원격 저장소의 내용을 가져오기
>>>>>>> issue3
~~~

충돌이 일어난 부분이 표시되고 그 부분은 일일이 확인 해서 수정 한후 commit 한다.

~~~bash
원숭이도 이해할 수 있는 Git 명령어
add: 변경 사항을 만들어서 인덱스에 등록해보기
commit: 인덱스의 상태를 기록하기
pull: 원격 저장소의 내용을 가져오기


$ git add myfile.txt
$ git commit -m "issue3 브랜치 병합"
# On branch master
nothing to commit (working directory clean)
~~~


### 60. 개발시 어려움이 많거나 풀리지 않을경우 어떤 해결방법을 사용하는가? 

- 공식 문서 참조.
- MDN,  w3cshool 문서 검색.
- google 검색.
- github 검색.
- 실수 노트 작성. 해결하지 못한 부분과 만약 해결 했다면 해결한 내용을 기록.

### 61. 배워나가는 기술을 공유하거나 기록하는 것들이 있는지? 

깃 허브를 통한 daily commit으로 하루하루 공부한 내용을 정리하여 업로드 하였다.

### 62. 본인이 생각하는 가장 적절한 커밋단위는 ? 

커밋은 할수 있으면 자주, 작은 단위로 하는것이 좋다고 생각한다. 커밋은 "마무리된 작업의 작업이력을 남기는 작업" 으로 볼수 있지만, 이후에 진행 된 부분이 잘못 된 방향으로 가고 있다면 다시 되돌리기위한 수단이라고 볼수 있다. 그렇기 때문에 함수 완성 단위로 커밋을 하게 되면 프로젝트의 방향이 잘 못 되어 다시 돌아가도 많은 피해 없이 새로운 작업을 할수 있기 때문이다. 


### 18. github을 통해서 협업과정을 거치는 과정을 branch를 중심으로 설명하기. 

**github flow**

master branch를 출시 하기 전까지 절대 merge 하면 안된다.

1. develop branch 생성 : 제품 출시 전 모든 작업(개발, 테스트, 배포 등)을 수행하는 branch
2. 팀원 모두 각자 작업할 branch 생성 : 기능 단위  또는 더 작은 단위로 branch를 생성하여 각자 여러 작업(개발, 테스트 등)후 개발한 기능을 develop branch에 merge한다. 작업이 끝난 branch는 삭제 한다. branch의 단위를 팀원 끼리 협의를 통해 정할수 있다.

### 11. es6에서 본인이 생각할때 지난번대비 개선된 점은 무엇이고, 왜 그렇게 생각하는지? 

es6가(es2015) 기존 es5보다 개선된 점은 함수 scope 에서 let, const를 통해  block scope을 사용 한다는 점이다. 기존 javascript에서는 var를 사용 하면서 변수의 재 정의 및 재 할당이 가능 했다. 함수 scope을 사용하면서  메모리 낭비가 심했던 var을 대신하여  let, const을 통해 성능을 개선 하였다. 이외에도 class, spread operator, arrow function 등 기존 자바스크립트의 객체를 생성하는 방식, this를 바인딩 방식을 더욱 편리하게 사용할수 있도록 개선 했다고 생각한다.

### 17. 문제가 있을때 본인이 가장 자주 사용하는 디버깅 과정은 무엇인가? 



### 32. ecmascript란 무엇인가? 

**ECMAScript**는 [자바스크립트](https://developer.mozilla.org/ko/docs/Web/JavaScript)의 토대를 구성하는 스크립트 언어이다. ECMAScript는 [ECMA International](http://www.ecma-international.org/) 표준화 기구에 의해서 **ECMA-262 및 ECMA-402 스펙**에서 표준화되었다.

**ECMAScript는 자바 스크립트를 이루는 코어(Core) 스크립트 언어**로, 웹 환경에서만 사용 되는 언어가 아니다. 웹 환경은 ECMA 스크립트를 사용하는 하나의 환경이다.  예를들어 웹 브라우져 환경에서는 BOM(Browser Object Model)과 DOM(Document Object Model)이 그 확장성이 되겠다. 이러한 확장성들은 EMCA 스크립트의 문법과 기능에 맞춰 기능의 확장을 가능게 한다. 자바스크립트의 document 객체가 좋은 예이다. 다른 환경으로는 node.js, Adobe Flash, MongoDB, CouchDB  등이 있다

ECMA 스크립트는 기본적으로 언어의
- 문법(Syntax)
- 데이터 타입(Type)
- 구문(조건문, 반복문 등)(Statement)
- 키워드(Keyword)
- 예정 키워드(Reserved Word)
- 연산자(Operator)
- 객체(Object)
  등을 규정짓는다. 

이는 어떤 ECMA 스크립트 호스트 환경에서든 동일하다.

**참조** 
- https://developer.mozilla.org/ko/docs/Web/JavaScript/%EC%96%B8%EC%96%B4_%EB%A6%AC%EC%86%8C%EC%8A%A4
- https://muckycode.blogspot.kr/2015/01/javascript.html