자바스크립트에는 지연처리를 시킬 수 있는 몇개의 메서드가 있는데, 대표적으로 setInerval과 setTimeout이 있다.

이 두개의 메서드는 기본적인 동작은 조금 다르지만 일정시간 이후에 코드를 실행해준다는 공통점이 있는데 지연처리를 할때 내부적으로 조금 다르게 동작한다.

우선적으로 자바스크립트에는 이벤트 큐라는 개념이 있는데 자바스크립트는 기본적으로 싱글 스레드환경에서 동작한다.

싱글 스레드 환경에서 비동기 로직과 이벤트를 제어하기 위해서 이벤트 큐를 사용하는데
이벤트 큐로 처리하는 것들은 Click Event와 같은 자바스크립트로 정의된 이벤트를 Listening 할때와 비동기 로직들(Ajax Callback, setTimeout, setInterval)을 처리할때 이벤트 큐에 넣고 순차적으로 처리한다.

이벤트 큐의 개념을 아주 단순화하여 코드로 표현하면

```javascript
var event_queue,
     event;

while(true){
     if(event_queue.length){

         event = event_queue.shift();
         event();
     }
}
```

와 같은 코드로 아주아주 단순화 시킬 수 있다(물론 실제 로직은 아주 복잡할 것이다.)

위 코드처럼 event_queue에 무언가 push되었는지 리스닝하다가 있는 경우 해당 이벤트를 실행해준다.(사실 이벤트를 실행한다기 보다는 이벤트의 콜백을 실행해주는 것일것 같다.)

이제 본론으로 돌아가서 setInterval과 setTimeout에 대해 이야기해보자.

setInterval같은 경우는 이벤트 큐에 무언가 있어도 우선처리되도록 설계되어 있다.

만약 이벤트큐에 20개의 요소가 들어가 있는데 setInterval 메서드의 지연시간이 다되어 callback이 이벤트 큐에 추가되어 한다면

현재 처리중인 이벤트가 끝난 후 최대한 빨리 실행 시켜줘!

라는 느낌으로 처리된다.

이와 관련하여 잡큐(Job queue)라는 개념이 있는데 잡큐가 바로 setInterval의 처리와 비슷한 경우이다.

물론 setInterval이 실제적으로 잡큐로 처리되는지는 찾아봐야 한다.

이렇듯 setInterval의 지연은 다른 이벤트보다 우선적으로 처리되도록 설계되어 있다.

이에 반하여 setTimeout은 이벤트 큐의 규칙을 아주 잘따르는 편이다.

지연시간이 되면 그때 이벤트 큐에 처리요소가 추가되고 자신의 순서가 오면 실행된다.

이런식으로 처리되기때문에 setTimeout과 setInterval 모두 실제 명시해준(파리미터로 전달해준) 시간과 완전히 동일한 Interval에 실행되는 것은 아니다.

그리고 다른 팁으로 비동기 이벤트에서 DOM을 제어해야하는 경우가 있는데

```javascript
Type A
function as_dom(){
     setInterval(function(){
        var target = document.getElementsById(“header”);
        target.style.width = “20px”;

          setInterval(function(){
               target.style.height = “30px”;
          }, 0)
     }, 0)
}

Type B
function as_dom(){
     setInterval(function(){
        var target = document.getElementsById(“header”);
        target.style.width = “20px”;
        target.style.height = “30px”;
     }, 0)
}
```

위와 같이 두가지 타입의 코드가 있다.

Type A는 비동기 이벤트가 2개이기 때문에 이벤트 큐가 2개이다.
Type B는 비동기 이벤트가 1개이기때문에 이벤트 큐가 1개이다.

그리고 브라우저에서 화면을 그릴때 리플로우와 페인트가 있는데
해당 요소들은 UI 쓰레드의 작업이 끝난 후 실행된다.

Type A와 같은 경우는 리플로우와 페인트가 각각 2번씩 실행된다.

2개의 큐에서 각각 일어나기 때문이다. 이는 성능에 영향을 줄 수 있다(물론 저렇게만 하는 경우에는 영향이 적겠지만 저런 코드가 많아지면 좋지 않다.)

그리고 Type B는 해당 UI 쓰레드가 끝나고 한번에 반영되기 때문에 리플로우와 페인트가 한번만 반영된다.

