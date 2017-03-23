# Async-Await

Asnyc-Await은 Promise를 기반으로 비동기 코드를 동기 코드처럼 짤수 있도록 도와주는 문법이다.

```javascript
async function deleteUserById(){
  const userData = await getUserData();
  await deleteUser(userData.userID);
}
```

위 함수를 보면 function Keyword 앞에 async Keyword가 들어가있다.

그리고 Statment를 보면 await구문이 쓰인것을 볼 수 있다.

async function은 항상 Promise를 return하도록 되어 있는데, 만약 async function에서 return 구문을 사용하면 resolve 값으로 해당 return 값이 넘어가게 된다.

await keyword는 담당하는 함수호출이 리턴하는 Promise가 resolve 될때까지 함수 호출을 중단하는 역할을 맡는다.

만약 값 할당 구문이 같이 있다면 resolve 된 값을 할당시켜준다.

async await은 Promise와 Gennerator의 Syntax Sugar라고 하는데 실제로 정말 Syntax Sugar인지, 아니면 구현적으로 다른 부분이 있는건지는 추가로 살펴봐야 할것 같다.

```javascript
var getAPI = () => {
  return new Promise((resolve, reject) => {
  	setTimeout(_ => {
    	resolve(4000);
    }, 3000);
  });
};

async function bar() {
  return await getAPI()
}

async function foo() {
	return await bar();  
}

foo().then(a => console.log(a)); // 4000
```