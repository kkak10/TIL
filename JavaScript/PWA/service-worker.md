# Service Worker(빠르고 안정적인 WEB)

## PWA

설치없이 개발자는 WEB을 개발하듯이 개발하지만, 사용자는 APP을 사용하는듯한 

경험을 할 수 있는 기술.

## 기존 Mobile WEB의 Pros and Cons

Pros
 - URL
 - 무설치
 - 많은 기기에서 사용가능

APP 대비적인 Cons
 - UX(대체제: Manifast + Service Worker + Push)
 - 성능(대체제: Service Worker + AMP)
 - 지원불가한 기능들

기본적인 동작방식

기본적으로 Background Service

페이지에서 HTTP 요청을 하면 중간에 Service Worker가 요청을 가로채서 자신이 Caching하고 있던 데이터를 Response로 전달.

Safari와 Edge에서는 아직 미지원

Event-base Worker
어떤 파일을 요청하면 fetch 이벤트가 발생하고 그러면 service worker가 동작.

- install
- client matching
- Functional event handling
 - oninstall
 - fetch
 - push
 - notificationclick
 - sync
 - etc......
- caching
 - pre-caching: oninstall
 - dynamic caching: onfetch
- serve on https
- fetch API / Stream API

## 3 Status
Install

Wait

Activate

## keywords
 - VAPID


push subscribe을 Service Worker 코드에서 해야함.