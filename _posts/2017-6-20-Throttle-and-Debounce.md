---
layout: post
title: 스로틀(throttle)과 디바운스(debounce)
tags: [Javascript, Lodash, Underscore]
---

*["자바스크립트 코딩 면접에서 알고 있어야 할 3가지 질문"](https://joshua1988.github.io/web_dev/javascript-interview-3questions/)* 포스팅을 보고 **디바운싱**(debouncing)에 대한 부분에서 무릎을 탁!!! 하고 쳤습니다.  
scroll과 resize 이벤트를 사용하면서 '이거 로직이 복잡하면 서버 부하가 걸릴 것 같은데….'라고 의문을 몇 번 던진 적이 있었는데, 이 부분을 시원하게 뚫어주었습니다.

디바운스(debounce)를 검색해보니 스로틀과 함께 소개하고 있어 스로틀과 디바운스에 대하여 짚어보겠습니다.

## throttle

> 밀리세컨드마다 실행의 흐름을 일정하게 유지한다.

스로틀 또는 스로틀링(throttling)은 로직 실행 주기를 만드는 함수라고 이해하면 됩니다.  
밀리세컨드 단위로 시간을 설정하면 스로틀에 넘긴 콜백함수는 설정한 시간 동안 최대 한 번만 호출됩니다.

## debounce

> 갑작스러운 이벤트(키 입력 등)를 하나로 그룹화합니다.

디바운스는 과다한 이벤트 실행을 방지하기 위해 몇 가지 함수를 한 개의 그룹으로 묶고 특정 시간이 지난 후에야 호출될 수 있도록 구조화하는 것입니다.  
아래는 스로틀과 디바운스를 적용했을 때의 예시입니다.

<p data-height="736" data-theme-id="dark" data-slug-hash="RgprJX" data-default-tab="result" data-user="moonspam" data-embed-version="2" data-pen-title="The Difference Between Throttling and Debouncing" class="codepen">See the Pen <a href="https://codepen.io/moonspam/pen/RgprJX/">The Difference Between Throttling and Debouncing</a> by moonspam (<a href="https://codepen.io/moonspam">@moonspam</a>) on <a href="https://codepen.io">CodePen</a>.</p>


## with Lodash/Underscore

자바스크립트 유틸리티 라이브러리중에 유사한 [Lodash](https://lodash.com/)와 [Underscore](http://underscorejs.org/)에서도 이 두기능을 제공하고 있습니다.

### throttle

- [lodash](https://lodash.com/docs/4.17.4#throttle)
- [underscore](http://underscorejs.org/#throttle)

### debounce

- [lodash](https://lodash.com/docs/4.17.4#debounce)
- [underscore](http://underscorejs.org/#debounce)

아래는 **Lodash** 또는 **Underscore** 라이브러리로 적용했을 때의 예시입니다.

<p data-height="736" data-theme-id="dark" data-slug-hash="dRvMWo" data-default-tab="result" data-user="moonspam" data-embed-version="2" data-pen-title="The Difference Between Throttling, Debouncing, and Neither (lodash)" class="codepen">See the Pen <a href="https://codepen.io/moonspam/pen/dRvMWo/">The Difference Between Throttling, Debouncing, and Neither (lodash)</a> by moonspam (<a href="https://codepen.io/moonspam">@moonspam</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async="async" src="//codepen.io/assets/embed/ei.js"></script>

## 참고 및 출처

- <https://joshua1988.github.io/web_dev/javascript-interview-3questions/>
- <https://hyunseob.github.io/2016/04/24/throttle-and-debounce/>