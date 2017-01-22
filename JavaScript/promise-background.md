커뮤니티에서 Promise에 대한 댓글을 보다보면 간혹 Promise에 대해서 잘못 이해하시는 분들이 계신다.

대부분 Callback Hell에 해결에 대해 묻는 글에 Promise를 언급하시는데 이는 잘못된 접근이다.

물론 Promise를 사용하면 Callback Hell을 간접적으로 해결할 수 있긴하다.(이는 가독성에 대한 문제이다.)

하지만 Promise가 길어지면 결국 Callback Hell과 별로 다른게 없는 코드가 된다.

Promise가 나온 배경을 이해하는게 중요한데 Promise는 JavaScript의 비동기 Context를 해결하기 위해서 나왔다.

Promise가 나오기 전에 (물론 Promise는 1970년대부터 존재하던 패턴이다.) 일반적으로 사용하던 Callback 패턴에 대한 신뢰성을 해결하고 비동기 컨테이너로서 나오게 되었다.

조금 더 신뢰할 수 있고 공통적으로 사용할 수 있는 비동기 컨테이너다.

공통적인 에러 처리와 쉽게 이해할 수 있는 호출 순서, 그리고 비동기 호출의 동시성을 보장해주는 컨테이너,

그리고 비동기 상태를 조금 더 쉽게 제어하기 위해서 나왔다.

Callback Hell과는 아무 상관이 없다고 생각하고 접근하는게 올바른 접근법이다.