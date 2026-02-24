---
layout: post
title: API 쉽게 이해하기
tags: [API, Javascript]
---

개발자와 회의하다 보면 `API`라는 단어를 종종 듣게 됩니다. 공부 겸 `API`란 무엇인지 쉽게 풀어 정리해 보겠습니다.

## API란?

**위키백과**에 설명하고 있는 `API`의 사전적 정의는 아래와 같습니다.

> API(Application Programming Interface, 응용 프로그램 프로그래밍 인터페이스)는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다. 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다.

무슨 내용인지 전문가가 아닌 이상 이해하기 쉽지 않습니다. 레스토랑을 예로 들어보겠습니다.

### 레스토랑에서…

![animation](https://media.giphy.com/media/3o6ZsYRW7iRixVPeRq/giphy.gif)

1. `손님`은 `웨이터`를 불러 <u>음식을 주문</u>합니다.
2. `웨이터`는 `셰프`에게 <u>음식을 요청</u>합니다.
3. `셰프`는 요리가 끝난 후 `웨이터`에게 <u>음식을 전달</u>합니다.
4. `웨이터`는 `손님`에게 주문한 음식을 제공합니다.

![Imgur](https://i.imgur.com/GjZMfdJ.png)

위 상황에서 `손님`은 메뉴에 있는 음식을 요청하는 **응용 프로그램**이라면 `셰프`는 요리를 만들어 전달하는 **운영 체제**라고 할 수 있습니다. 이 둘을 연결해주는 `웨이터`가 바로 **API**입니다. 조금 더 생각해보면 `메뉴`는 API의 **인터페이스**라고 할 수 있습니다.

## API 어떻게 사용할까

API를 어떻게 쓰는지 예를 들어 작성해 보겠습니다.  
먼저 무료로 사용할 수 있는 오픈 API 사이트 몇 곳을 소개합니다.

> API를 무료로 이용할 수 있다고 해도 본인 인증할 수 있는 키가 필요하며, 몇 번까지 **무료** 초과 이용 시 **유료**인 API도 있습니다.

- 공공데이터포털 : <https://www.data.go.kr/>
- Kakao Developers : <https://developers.kakao.com/>
- Naver Developers : <https://developers.naver.com/>
- OpenWeatherMap : <http://openweathermap.org/>

날씨 정보를 제공하는 **OpenWeatherMap**의 `API`를 이용해 런던의 날씨 정보를 알아보겠습니다.

## API 사용하기

레스토랑의 이야기를 토대로 설명해보자면, 런던의 현재 날씨를 보여주는 **응용 프로그램**을 만들기 위해 OpenWeatherMap 이라는 **운용 체제**의 `API`를 통해 정보를 제공 받게 됩니다.

### jQuery 예제

현재 날씨 정보를 받아오기 위한 API 주소는 아래와 같은 형식으로 구성됩니다. [🔗]

[🔗]: http://openweathermap.org/api

``` javascript
var api = "//api.openweathermap.org/data/2.5/weather?q=지역&appid=인증키";
```

`jQuery.getJSON()`를 이용하여 요청한 API 정보를 HTML 내 출력하는 예시를 작성해 보았습니다.

<p data-height="300" data-theme-id="0" data-slug-hash="baLVxM" data-default-tab="js,result" data-user="moonspam" data-embed-version="2" data-pen-title="OpenWeatherMap API Sample" class="codepen">See the Pen <a href="https://codepen.io/moonspam/pen/baLVxM/">OpenWeatherMap API Sample</a> by moonspam (<a href="https://codepen.io/moonspam">@moonspam</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## 정리

`API`는 어렵고 복잡한 과정을 쉽게 전달받을 수 있도록 만들어주는 다리 역할을 해줌으로써 작업을 빠르고 간결하게 진행할 수 있습니다. 👍👍👍

![easy](https://media.giphy.com/media/ZgzrLrlGf5ENW/giphy.gif)