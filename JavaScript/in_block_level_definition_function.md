vascript 책을 보다가 처음 알게된 사실인데

함수안에 있는 블록에서 선언된 함수를 부모 함수에서 실행할때의 동작에 대한 내용이다.

내가 기본적으로 알고 있던 상식으로는 부모 함수의 블록 안에서 선언된 함수여도 결국 호이스팅에 의해서 끌어올려져서 부모 함수 내에서 실행시 global 레벨에 동일 이름의 함수가 있더라도 부모 함수내에서 끌어 올려진 함수가 실행되게 된다. 라는게 내가 알고 있던 내용인데 이게 실행 환경에 따라서 완전히 달라진다.

```javascript
function good(){
    console.log("haha");
}

function test(bool){
    if (bool) {
        function good(){
            console.log("gg");
        }

        good();
    }

    good();
}

test(true);
```

위 와같은 코드가 있다.

test라는 함수를 실행할 경우 test내의 block 레벨에 선언된 good이라는 함수가 호이스팅에 의하여 끌어올려지고 결과는 console에 “gg”가 두번 찍히게 된다.

여기까지는 내가 알고 있던 상식과 동일하다.

하지만 만약에 “use strict”;를 선언하면 결과는 완전히 달라진다.
```javascript

"use strict";

function good(){
    console.log("haha");
}

function test(bool){
    if (bool) {
        function good(){
            console.log("gg");
        }

        good();
    }

    good();
}

test(true);
```

위와 같이 “use strict”를 선언해버리면 엄격한 문법모드에서는 저런 block레벨 내에서 함수선언하는것을 인정하지 않는다.

그래서 처음 호출된 block 레벨 안쪽에 good()은 “gg를 출력하고” 밖에 있는 good()은 글로벌 레벨에 있는 “haha”를 출력하게 된다.

처음 알게되었는데 놀라운 결과이다.

기본적으로 블록 스코프로 선언될 수 있는건 try~catch의 catch문과 with문과 eval문, 그리고 ES6에서 공식 표준이 된 let의 사용일때로 알고 있었는데 저런식으로도 될 수 있다는것이 신기하다.

물론 저 방법은 권장하지 않는방법이라 lint 도구를 돌리면 오류가 표출될 것이다.
그리고 브라우저에 따라서 아에 오류를 표출하는 경우도 있는것 같다.
