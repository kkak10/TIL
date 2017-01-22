MA2015에서 추가된 개념중에 Iterable이라는 개념이 있다.

이 개념은 Iterable이라는 말 그대로 반복 가능한 객체를 뜻한다.

ECMA2015에서는 Symbol을 이용한 프로토콜이 추가되었는데,

이는 프로토콜 기반의 프로그래밍을 가능하게 한다.

덕타이핑을 이용하여서 Symbol중 Symbol.iterator를 key로 가지고 있는 객체를 iterable한 객체로 본다.

Symbol.iterator을 key로 가지고 있는 객체는 해당 Key의 함수를 실행할 경우 next함수를 구현한 iterator를 리턴해야 한다.

예제 코드를 살펴보자.

```javascript
{
	let iterable = {
	  [Symbol.iterator]() {
	      let [iteratorData, step] = [[1,2,3,4,5], 0];

	      return {
	          next: () => {
		          let [return_data, is_last] = [iteratorData[step], iteratorData.length === step];
		          step++;

	            return {
	                "value": return_data,
	                "done": is_last
	            };
	          }
	      };
	  }
	};

	for(let data of iterable){
	    console.log(data);
	}
}

```

1. iterable 변수는 Symbol.iterator를 구현하고 있기 때문에 iterable한 변수이다.

2. Symbol.iterator 메서드에서는 next 메서드를 구현한 iterator를 리턴해야 한다.

3. Symbol.iterator 메서드의 구현 내용은 iteratorData라는 배열의 내용을 next 메서드를 실행 할때마다 하나씩 리턴해주는 메서드이다.

4. next 메서드는 {value : 값, done: Boolean}을 리턴해야하는데 이는 규칙이다.

5. 하단의 for..of문은 iterable한 값을 위한 syntax인데 of의 오른쪽에 iterable한 값을 넣고 왼쪽의 변수로 받을 수 있다.

6. for..of 문에서는 리턴 되는 done 값이 true일 경우 loop를 멈춘다.

기본적인 동작은 이렇게 진행된다.

JavaScript의 빌드인 Object들 중에서도 iterable한 Object들이 있다.

예를 들면 String, Map, Set, Array, object literal 같은 Object들은 iterable하다

따라서 for..of문으로 동작 시킬 수 있다.

### String

```javascript
{
	let str = "Hello Iterator";

	//str 문자열이 문자 1개씩 리턴된다.
	for(let char of str){
		console.log(char);
	}
}
```

### Array
```javascript
{
	let list = [1, 2, 3, 4, 5];

	//list 변수의 하나의 원소가 리턴된다.
	for(let item of list){
		console.log(item);
	}
}
```

위와 같이 빌드인 Object 들도 iterable하다.
