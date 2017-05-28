## Code Test

1.  Code Test
- 자바스크립 코드가 잘 동작되는지 확인하는것.

2. Unit Test
- 자바스크립트의 최소단위(함수)를 테스트 하는 것.
- 사람이 직접 함수를 호출하는 것이 아니고, 프로그램을 통해서 자동으로 동작하게 하는것.


3. Mocha 테스트 프레임워크, chai와 같은 assertion 라이브러리를 추가해서 사용한다.

~~~
const assert = chai.assert;
// 어떤 테스트 인지 설명
describe("utility selector test",function(){


    //Create Test
    it("should be document when use $", function(){

        assert.equal(utility.$,document);
    });

    it("should be document when return dom element", function(){
        let element = utility.$selector("#mocha");
        assert.equal(element.id,"mocha");
    });

});
~~~

4. 비동기 테스트를 조심해야한다.

**예**

~~~
const assert = chai.assert;

//비동기 테스트는 모든 수행이끝나고 동작된다
// done 메소드 필요, reqListener는 모든 동작이 끝나고 동작한다. done 는 테스트의 끝은 여기다 라고 명시하는 것이다. 비동기에서는 it이 먼저 실행되고 그 다음 reqListener가 실행 되기 때문에 조심해이햔다
describe("utility Ajax test",function(){

    //Create Test
    it("should get Album data when receive runAjax request and response", function(done){
        const url = "./albumListData.json";
        let reqListener = function(event){
            let resData = JSON.parse(event.currentTarget.response);
            resData.forEach((val)=>{
                assert.equal(Array.isArray(val.playList),Array.isArray([]));
            });
            //done은 비동기가 정상적으로 완료 되었을때 실행시켜 주면 된다

            done();
        };
        utility.runAjax(reqListener,"GET",url);

    });
});
~~~

비동기 테스트 시 주의해야 할점은 `done()` 함수의 사용이다. 비동기이기 때문에 