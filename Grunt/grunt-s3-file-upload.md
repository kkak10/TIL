Sprite 파일을 CDN으로 빼게 되면서 Sprite를 새로 빌드 할때마다 CDN에 전송해야 할때가 있었는데,

grunt-aws-s3를 쓰게 되었다.

Docs대로 설정하고 서울 리전에 있는 S3에 전송했는데 안되는 것이다.

그래서 도쿄 리전에 있는 S3로 전송했더니 전송이 잘되었다.

궁금해서 찾아보니 서울 리전에 전송할때는 시그니처 설정을 해줘야 한다고 한다.

시그니처를 v4로 설정하고 전송하니 서울리전에서도 정상적으로 동작했다.