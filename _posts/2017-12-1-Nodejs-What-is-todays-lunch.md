---
layout: post
title: 점심 뭐 먹지? 제작기
tags: [Javascript, Nodejs]
---

인간으로 태어나 가장 큰 고민거리인 **오늘 점심 뭐 먹지?** 에 대한 고민을 해결하기 위해 만들어 보았습니다.  
사실 회사에서 점심 고르는 게 힘들어서 만들었다는 후문이…. 🤣

![yam](https://media.giphy.com/media/l0HlDgg6jypfnVejC/giphy.gif)

<http://lunch.moonspam.me/>

## Prototype

바야흐로 3년전 cafe24 호스팅을 이용 중이어서 PHP로 간단하게 만들어보자며 굳은 의지와 함께 틈틈이 코딩했습니다.  
나름 핵심기능이었던 부분은 음식점에 확률값을 넣는 부분이었습니다.

<script src="https://gist.github.com/moonspam/42f33b4a8f7bd460c50d6c96f3521a29.js"></script>

## Beta

음식점 추가/수정을 하면 확률함수에 리스트를 넣고 빼는 게 반복되다 보니 음식점 정보에 확률값을 넣는 것으로 수정하였습니다.

<script src="https://gist.github.com/moonspam/5263650354b222dde079959c7726f7f3.js"></script>

## v1.0

<https://github.com/moonspam/Node-Study-lunch>

음식점리스트를 누르면 정보가 보이게끔 구현하고 싶었고 Node.js도 겉핥기로 배웠지만 써먹어 보자는 의지와 함께 틈틈이 키보드를 두들겼습니다.  
버전업한 내용은 나름 그럴싸합니다. 😥

- node.js를 이용하여 SPA(Single Page Application) 구현
- npm, bower를 통한 패키지 관리
- csvtojson 패키지를 이용한 csv ==> json 변환

## 후기

초기 제작했을 때는 구글 검색으로 코드를 붙여넣기 신공이 대부분이었다면, `v1.0`에서는 전체적인 구조를 이해하면서 코딩을 한 부분과 [Heroku](https://www.heroku.com/)를 통해 배포해 보는 경험을 한 경험이 아주 조금이지만 한 발짝 걸었다는 기분을 들게 하였습니다.
