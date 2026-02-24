---
layout: post
title: Vue.js 타이머 만들기
tags: [Vuejs, Javascript, Firebase]
---

아내의 수유 시간 체크와 업무 중 자리 비운 시간을 체크하기 위해 토이 프로젝트로 `타이머`를 만들어 보았습니다.  
*간단하고 쉽게 만들자* 컨셉에 맞춰 레트로 스타일로 만들었습니다. 이름하여 **8비트 타이머!**

![8-bit Timer](https://user-images.githubusercontent.com/15099135/62300351-972c8880-b4b1-11e9-966a-1363ebee75e6.jpg)

<https://timer.moonspam.com/>

## 일단 만들어 보자

신식이 최고야!라고 외치는 나의 성격 탓에 틈틈이 시도해보고 있는 vue.js를 이용해 타이머를 만들어보기로 했습니다.

### Vue CLI

Vue.js에서 공식 지원하고 있다는 [Vue CLI](https://cli.vuejs.org)를 통해 쉽게 프로젝트 생성을 할 수 있었습니다.  
3.x 버전부터 GUI 인터페이스로 프로젝트를 관리할 수 있게 바뀌었더군요.

설치와 실행은 간단합니다.

#### 설치

```bash
npm install -g @vue/cli
```

#### GUI 실행

```bash
vue ui
```

![Vue UI](https://user-images.githubusercontent.com/15099135/64396788-c7aba780-d099-11e9-8592-fa1fec03b0b6.png)

Vue 프로젝트 매니저를 통해 기본 템플릿을 설정하고 서버를 구동하며 시작이 반이라고 반이나 했다!라는 생각도 잠시... setInterval로 시간을 1초씩 차감하면 될 거라고 생각했지만 어떻게 구성하여 풀어나가야 할지 막막했습니다.

이미 제작된 여러 타이머 소스들을 보던 와중 쉽고 이해하기 편한 예시를 찾았습니다.  
매번 이런 소스들을 볼 때마다 이렇게 쉽게 풀 수 있는 걸 어렵게 고민하고 있었을까? 반성하게 됩니다.

아래는 제가 만든 타이머의 큰 틀을 잡아준 코드입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="js" data-user="joshuaward" data-slug-hash="MMNVrm" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Vue: Timer ⇢ WIP">
<span>See the Pen <a href="https://codepen.io/joshuaward/pen/MMNVrm/">
Vue: Timer ⇢ WIP</a> by Joshua Ward (<a href="https://codepen.io/joshuaward">@joshuaward</a>)
on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 예쁘게 꾸며보자

조금 코딩하고 바로 꾸미기에 돌입! 주로 모바일로 볼 거라 페이지 레이아웃을 `display: flex`를 이용해 제작했습니다. flex를 이용해 한땀 한땀 width 값을 넣지 않아도 알아서 조절되고 여러 줄을 써서 가운데 정렬했던 부분을 한 줄로 끝내는 등 편리한 부분이 많았습니다.  
언젠간 사용해봐야지 했던 [NES.css](https://nostalgic-css.github.io/NES.css/)도 적용하여 레트로 스타일 레이아웃 완성!

## 먼가 부족해

Vue.js 공식 문서를 보면서 꾸역꾸역 완성할 때 즈음 모바일에서 테스트해보니 추가하고 싶은 부분들 발생...  
작업 중에 추가한 기능들을 나열해 보았습니다.

- 전체 화면 구현 스크립트 추가 [🔗](https://developers.google.com/web/fundamentals/native-hardware/fullscreen/?hl=ko)
- 모바일에서 화면 안 꺼지게 해주는 기능 추가 [🔗](https://github.com/richtr/NoSleep.js/)
- 남은 시간 5분, 정각에 진동 울리기 추가 (iOS에서는 안됨)
- `dialog` 태그를 사용하기 위한 dialog-polyfill [🔗](https://github.com/GoogleChrome/dialog-polyfill)
- 파비콘을 쉽게 넣고 싶어서 찾은 favicon.io [🔗](https://favicon.io/)

## 후기

프로젝트를 진행하면서 `Vue.js`가 어떤 형태로 돌아가는지 수박 겉핥기 식으로 훑었지만 토이 프로젝트에 맞게 재밌게 코딩한 작업이었습니다.
