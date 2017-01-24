# Javascript Function Object

```javascript
var func = function foo(){};
```

Javascript Engine이 위와 같은 코드를 만나면 할당 연산자(=)를 기준으로 우항의 값을 계산하여 좌항에 할당합니다.

우항에 ```function``` 키워드가 있기때문에 Built In Function Object로 Function object를 생성해서 반환합니다.

```
아래부터는 Built In Object들은 (Math, Function, Object, Array 등등..) Object로 표기하고 실제 instance는 object로 표기합니다
```

해당 과정을 조금 더 자세히 알아봅시다.

  1. 빈 오브젝트를 생성합니다.

  2. func 변수에 빈 오브젝트를 넣습니다.
    ```javascript
    var func = {}
    ```

  3. func 오브젝트에 prototype을 설정합니다.
    ```javascript
    var func = {
      prototype: {}
    }
    ```

  4. func.prototype에 constructor를 설정합니다.
    ```javascript
    var func = {
      prototype: {constructor: {}}
    }
    ```

  5. func.prototype.constructor에 foo에 대한 참조를 설정합니다.
     이때 foo 함수는 메모리상에 올라가게 됩니다
    ```javascript
    var func = {
      prototype: {constructor: foo}
    }
    ```

  6. func.prototype에 ```__proto__```를 추가합니다.
	```javascript
	var func ={
      prototype: {constructor: foo, __proto__: null}
    }
    ```

  7. Object의 prototype에 연결된 Property로 object를 생성해서 ```__proto__```에      대입합니다.
	```javascript
	var func ={
      prototype: {constructor: foo, __proto__: {}}
    }
    ```
  8. Built In Function Object prototype의 property로 Function Object를 생성해서 ```func.__proto__```에 대입한다.
  ```javascript
	var func ={
      prototype: {constructor: foo, __proto__: {}},
      __proto__: Function Object,
    }
  ```

  9. func의 Property에 Function 관련 값들을 설정합니다.
 ```javascript
	var func ={
      arguments: null,
      length: 0,
      name: "foo",
      prototype: {constructor: foo, __proto__: {}},
      __proto__: Function Object,
    }
  ```

위 단순한 코드가 실행되는 과정이 이렇게나 복잡하다!

추가된 property를 하나하나씩 보자

``
arguments
``

arguments는 실제로 function이 호출될때 넘어가는 인자 값이 담기는 property이다.
기본값은 null이며 실제로 함수의 인자가 넘어올때 set된다

``
length
``

length는 함수가 parameter로 정의한 인자의 갯수를 말한다.
만약 ```javascript function (a,b,c){}```
이런 함수가 있다면 length가 3으로 설정된다.


``
name
``
name에는 기본적으로 함수의 이름이 들어가게 된다. 만약 익명 함수라면 대입되는 변수명이 name이 된다.

