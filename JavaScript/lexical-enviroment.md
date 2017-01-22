ECMA5(ECMA-262) 스팩에서 정의된 스팩인데 이전에는 이와 비슷하지만 다르게 설계되었다고 한다.

사실 이 부분은 일반적인 자바스크립트 개발자를 위한 내용이라기보다 자바스크립트 엔진 개발자들을 위한 스팩이라고 볼 수 있다.

엔진 개발자들이 해당 스팩에 맞춰서 구현해야하기 때문이다.

Lexical Enviroment는 Execution Context (이하 EC)와 관련이 있는데 Lexical Enviroment를 설명하기 위해서는 EC에 대한 개념이

필수적이기 때문에 EC를 먼저 설명하겠다.

EC는 이름 그대로 실행 환경인데 자바스크립트 런타임에서 엔진이 만들어주는 런타임 유효 범위라고 할 수 있다.

모든 자바스크립트 코드를 실행하기 위해서는 EC가 필요한데 코드를 실행하다보면 여러 Context가 만들어진다.

EC를 만드는 단위는 Global Code, Function Code, Eval Code 이렇게 3개인데 이런식으로 나누는 이유는 각각 코드를 초기화 할때

초기화 과정이 다르기 때문이다.

```javascript
ExecutionContext = {
     LexicalEnviroment: [Lexical Enviroment],
     VariableEnviroment: [Lexical Enviroment],
     ThisBinding: [object]
}
```

EC는 위와 같이 생겼다.

Value에 있는 []는 Type을 뜻한다.

EC 안에는 3개의 프로퍼티가 있는데 LexicalEnviroment, VariableEnviroment, ThisBinding 이다.

LexicalEnviroment와 VariableEnviroment는 LexicalEnviroment 타입을 가지고 ThisBinding은 object타입을 가지는데

LexicalEnviroment는 현재 실행 환경에 대한 변수나 참조에 대한 정보를 가지게 되는데 이는 곧 현재 Context에서 변수나 어떤 값들 (자원)을 가져올때 어디서 가져올 것이냐에 대한 답이다.

VariableEnviroment는 Lexical Enviroment와 같은 값이 들어가게 되는데 이 둘을 나눈 이유는 이후에 설명하도록 하겠다.

그리고 ThisBinding은 현재 Context에서의 This를 가르키게 된다.

```javascript
Lexical Enviroment = {
     enviroment record: {},
     outter enviroment reference: {}
}
```

Lexical Enviroment 타입은 위와 같은 형태이다.

enviroment record는 현재 Context에서 선언된 함수 혹은 변수들이 저장되는 공간.

outter enviroment reference는 현재 Context를 기준으로 외부 Context를 참조하는 공간이다. 만약 유효범위가 중첩되어 있을때 상위 유효범위를 참조할 수 있어야 하는데 상위 유효범위를 참조하기 위해서 존재하는 property이다. 그렇기 때문에 record가 아닌 reference라는 이름이 붙여졌다.

enviroment record는 Declartion와 object 두 가지 타입으로 되어 있는데

DeclartionEnviromentRecord는

```javascript
DeclartionEnviromentRecord = {
     a: 33,
     b: “Hello World"
}
```

와 같이 Object안에 실제 값들이 저장되어 있는 형태이고

ObjectEnviromentRecord는

```javascript
ObjectEnviromentRecord = {
     bindObject: [object]
}
```

와 같이 단순히 해당 Context 내부를 바인딩 해주는 역할만 하게 된다.

ES3까지는 DeclartionEnviromentRecord는 존재하지 않았는데 ES5 스팩에서 추가된 속성이다.

아마 현재 context의 값을 참조를 통해서 가져오는 형태가 성능상으로나 매커니즘상으로 부합하지 않다고 생각해서

DecleartionEnviromentRecord를 만들어서 바로 가져올 수 있도록 만든게 아닌가 싶다.

그게 문맥상이나 매커니즘상으로 훨씬 더 잘 맞는 형태이니깐.

만들어진 Context는 내부에서 Stack형태로 저장되게 되는데 먼저 생성된 Context가 아래에 쌓이게 되고 최근에 만들어진 Context가

위쪽에 쌓이게 된다. 그리고 함수실행을 마치거나 GC가 판단하여 Context를 쓸 일이 없게 되면 Context를 Stack에서 제거하게 된다.

엔진이 EC를 어떻게 만들어야 하는가에 대한 스팩이 Lexical Enviroment이다.

그래서 Lexical Enviroment와 EC는 깊은 관련이 있다.

위에서 Execution Context는 3가지 타입으로 나눠서 초기화 된다고 했다.(Global Code, Function Code, Eval Code)

먼저 Global Code 먼저 살펴보면,

```javascript
Global Execution Context
{
     LexicalEnviroment: GlobalEnv,
     VariableLexicalEnviroment: GlobalEnv,
     ThisBinding: window
}
```

위와 같이 생겼다.

GlobalEC의 LexicalEnviroment와 VariableLexicalEnviroment는 같은 GlobaleEnv를 참조하고 있다. (이 부분은 나중에 설명할 예정이다.)

그리고 ThisBinding은 window를 참조하고 있는데 브라우저의 전역 객체가 Window이기 때문이다.

만약 브라우저가 아닌 다른 환경(Node.js)일 경우에는 Global 같은 객체가 들어갈 것이다.

그리고 GlobalEC의 LexicalEnviroment가 참조하는 GlobalEnv를 보면

```javascript
 GlobalEnv
{
     ObjectLexicalRecord: {
          ObjectBinding: Window
     },
     OutterEnviromentReference: null
}
```

위와 같이 되어 있는데 ObjectLexicalEnviroment의 ObjectBinding 프로퍼티가 Window를 보고있는데 GlobalEC의 ThisBinding도 Window를 보고 있다.

저 ObjectLexicalEnviroment에 대해 이해하는 관점이 선언된 변수 혹은 함수를 가져오고 만드는게 아니고 미리 저 Window를 넣어놓고 그 후에 선언된 변수와 함수를 가져와서 저 ObjectLexicalRecord에 넣어주는 형태이다.

전자와 같이 생각하면 전역에서 변수를 선언했을때 전역객체의 프로퍼티에서 변수를 찾는 일이 불가능하다. 그렇기 때문에 후자와 같이 이해를 해야한다.

Global Code와 Function Code를 나눠서 EC를 생성하는 이유중에 하나가 될 수 있을 것같다. 물론 기본적으로 Global Code는 Base가 되는 EC가 되므로 다르게 Init하는게 맞다.

전역에서 변수를 선언하면 그 변수는 GlobalEnv의 ObjectLexicalEnviroment의 ObjectBinding에서 찾게되고 ThisBinding 역시 Window를 바라보고 있기때문에 전역에서 선언한 변수가 Global Object의 프로퍼티로 찾을 수 있게 되는 것이다.

OutterEnviromentReference는 Global이므로 상위 EC가 없다. 그래서 null이 들어간다.

그리고 다음으로 Function Code의 EC를 보도록 하자.

Function Code의 EC가 만들어 질때는 EC Stack에 Gloabel EC가 생성되어 있다.

```javascript
function sum(x, y){
     var result = x + y;
     var etc = function(){
         console.log(“good”);
     }

     function msg(){
          return result;
     }
}

sum(10, 20);
```

위와 같은 코드가 있다고 가정해보자.

자바스크립트 엔진이 sum(10, 20)을 만나면 Function에 대한 EC를 생성하게 된다.

그리고 우선적으로 this를 찾아서 바인딩 해주게 되는데 엔진이 this를 바인딩하는 메커니즘은 함수 실행시에 좌항을 보는 것이다.

만약 저 함수가 메서드로 실행된다면 메서드를 가지고 있는 객체가 this에 바인딩 될 것이고 new를 붙여서 실행한다면 해당 함수 자체가 this가 될 것이다.

위 코드에서는 좌항에 아무것도 없기 때문에 저 EC의 ThisBInding은 null이 될 것이다.

EC의 ThisBinding이 null일 경우에는 Global EC를 참조하게 된다.

브라우저에서는 Window가 될 것이다.

단, “use strict” 모드에서는 Global EC가 아닌 null이 그대로 들어가게 된다.

그리고 이제 EC의 Lexical Enviroment를 넣어줘야한다.

sum의 LE는
```javascript
{
     DecleartionEnviromentRecord: {

     },
     OutterEnviromentReference: null
}
```

위와 같이 구성되는데 첫번째로 파라미터로 들어오는 x와 y가 저 곳에 들어가게 된다.

```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20
     },
     OutterEnviromentReference: null
}
```

와 같은 형태가 될 것이고 그 다음으로는 함수 내부에 선언된 것들을 가져온다.

처음으로는 함수선언을 먼저 가져온다.

```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference
     },
     OutterEnviromentReference: null
}

```

그리고 함수 내부에서 사용할 수 있는 arguments를 세팅한다.
```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference,
arguments: Arguments Object
     },
     OutterEnviromentReference: null
}
```

그리고 선언된 변수들을 가져오게 된다.

그리고 함수 내부에서 사용할 수 있는 arguments를 세팅한다.

```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference,
arguments: Arguments Object,
result: undefined,
etc: undefined,
},
     OutterEnviromentReference: null
}
```

이렇게 되는데 undefined이고 msg는 Function Reference를 참조하는 이유는
함수 선언식과 함수 표현식에 대한 차이이다.

이 부분에 대한 설명은 생략하도록 하겠다.

자 이제 sum의 EC의 LE의 DecleationEnviromentRecord의 세팅이 완료 되었는데

이때 눈치가 빠른 사람은 아! 했을 수 있다.

이게 바로 호이스팅의 실체이기 때문이다.

런타임에서 실제적인 실행 전에 EC를 구성하는 단계에서 이런식으로 호이스팅이 일어나게 된다.

그리고 함수가 실행된다.

이제 OutterEnviromentReference를 세팅해준다.

OutterEnviromentReference는 스팩을 보면 가까운 상위 EC를 참조한다고 되어있는데 현재의 경우에서는 상위 EC가 Global EC이다.

그래서
```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference,
arguments: Arguments Object,
result: 30,
etc: undefined,
},
     OutterEnviromentReference: GloabalEC
}
```

와 같이 된다.

이제  var result = x + y;를 만나서 계산한 후에
```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference,
arguments: Arguments Object,
result: 30,
etc: undefined,
},
     OutterEnviromentReference: null
}
```

이 되고
```javascript
 var etc = function(){
         console.log(“good”);
     }
```

를 만나서
```javascript
{
     DecleartionEnviromentRecord: {
x: 10,
y: 20,
msg: Function Reference,
arguments: Arguments Object,
result: 30,
etc: Function Reference
},
     OutterEnviromentReference: null
}
```
이 된다.

이런식으로 Function을 실행할때 EC가 생성되는 과정을 보았다.

만약 이제 상위 EC에 있는 값을 사용하려 할때는 어떻게 하는가 ? 에 대해 궁금해 할 수있는데 이때 사용되는것이 OutterEnviromentReference이다.

이때 저 곳에 참조된 값을 사용하여서 상위 Scope로 찾아올라가게 된다.

이 과정이 바로 Scope Chain이다.
