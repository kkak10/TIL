# resource-router-middleware

## 개요
해당 모듈은 Node.js에서 RESTFUL API 작성시 쉽게 작성 할 수 있게 도와주는 라이브러리이다.

## 사용법
```javascript
import resource from 'resource-router-middleware';

export default resource({
    id : 'user',

    load(req, id, callback) {
        var user = users.find( user => user.id===id ),
            err = user ? null : 'Not found';
        callback(err, user);
    },

    index({ params }, res) {
        res.json(users);
    },

    create({ body }, res) {
        body.id = users.length.toString(36);
        users.push(body);
        res.json(body);
    },

    read({ user }, res) {
        res.json(user);
    },

    update({ user, body }, res) {
        for (let key in body) {
            if (key!=='id') {
                user[key] = body[key];
            }
        }
        res.status(204).send();
    },

    delete({ user }, res) {
        users.splice(users.indexOf(user), 1);
        res.status(204).send();
    }
});
```

## 기본 설명
위 사용법을 기반으로 설명하도록 한다.

기본적인 동작은 index 메서드를 제외한 모든 메서드는 load 메서드를 먼저 실행한 후 load 메서드의 리턴값을 Parameter로 받게되고 받은 인자를 기반으로 처리하여서 response를 던지면 되는 라이프 사이클을 가지고 있다.

## Q&A
### create, read, update, delete 메서드에서 load가 넘긴 값을 못가져올때
해당 부분은 "id" Property와 관련이 있는데 "id" Property에 설정한 String이 create, read, update, delete에서 받는 인자의 key값이 된다. 예를들어 id가 "person"이면 인자를 받는 부분도 "person"인지 확인해야한다. 그리고 Javascript는 Case senstive이기때문에 대소문자를 반드시 맞춰야한다.