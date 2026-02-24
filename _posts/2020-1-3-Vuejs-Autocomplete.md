---
layout: post
title: Vue.js 자동완성 만들기
tags: [Vuejs, Javascript]
---

2019년 목표였던 vue.js 다뤄보기를 하려고 노력한(?) 결과 프로젝트에서 검색창에 자동완성 되는 기능을 구현할 기회가 있었습니다.  
냉큼 작업이 가능하다고 질러놓고 돌아온 깊은 번뇌의 시간들... 어떻게 구성해야 할지 고민하다가 구글링을 통해 접근 방법을 찾았습니다. 그리고 찾아온 시련들...

<figure class="video">
  <iframe src="https://giphy.com/embed/O5NyCibf93upy" frameborder="0" allowfullscreen="true"></iframe>
</figure>

## 들여다보자

stackoverflow에서 찾은 [답변](https://stackoverflow.com/questions/52558770/vuejs-search-filter)을 토대로 큰 틀을 잡기로 하였습니다. 작업하면서 소소하게 덧붙인 부분들을 간단히 정리해보자면 아래와 같습니다.

- `장`을 입력할 때, `ㅈ`, `자`, `장` 단계별로 검색 값이 바로 나오게 구현
- 단어와 단어 사이 공란이 있어도 검색되도록 구현
- `\` 입력 시 vue.js 오류로 인해 특수문자 입력 방지
- 키보드 방향키로 자동완성 리스트 이동하도록 구현

## 자세히 보자

한글을 작성할 때 실시간 바인딩하는 부분과 장문 지원, 특수문자 입력 방지 이 3가지 부분을 가지고 고민을 많이 했습니다.

### 1. 한글은 v-model 대신 v-on:input으로

신나게 코딩 후 키보드를 타이핑하는 순간 어안이 벙벙해진 나의 모습...  
이유는 `장ㄷ`을 입력해야 `장`의 검색 값이 출력되기 때문이다.

가이드 문서를 뒤져보니 vue.js 공식문서를 참고하면 [아래 내용](https://kr.vuejs.org/v2/guide/forms.html#%EA%B8%B0%EB%B3%B8-%EC%82%AC%EC%9A%A9%EB%B2%95)을 안내해주고 있었다.

> IME (중국어, 일본어, 한국어 등)가 필요한 언어의 경우 IME 중 v-model이 업데이트 되지 않습니다. 이러한 업데이트를 처리하려면 input 이벤트를 대신 사용하십시오.

그러하다... 한국어 입력할 땐 `v-model` 대신 `v-on:input`을 사용해야 한다는 사실!

### 2. 특수 문자를 넣으면 오류를 뿜뿜

자체 테스트 중 `\` 문자를 입력하면 아래 오류를 뿜으며 페이지가 뻗어 버립니다.

> *SyntaxError: Invalid regular expression: /\/: \ at end of pattern*

이 아이를 해결하기 위해 정규식을 사용하여 한글, 영문, 숫자만 자동완성에 반응하도록 처리를 합니다.

```javascript
const reg = /[^가-힣ㄱ-ㅎㅏ-ㅣa-zA-Z0-9|\s]/.test(str);
if (reg === false) {
  this.isActive = true;
} else {
  this.isActive = false;
}
```

### 3. 키보드 키입력을 간단하게

vue.js에 내장되어있는 [키 수식어](https://kr.vuejs.org/v2/guide/events.html#%ED%82%A4-%EC%88%98%EC%8B%9D%EC%96%B4)를 사용하면 간단하게 키보드 이벤트 핸들링을 할 수 있습니다.  
추가적으로 몇가지 별칭을 제공하고 있어 해당 별칭을 이용해 리스트 이동을 구현하였습니다.

- `.enter`
- `.tab`
- `.delete` (“Delete” 와 “Backspace” 키 모두를 캡처합니다)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

## 끝으로

키보드 입력 처리로 코드가 길어진 느낌이지만 vue.js를 이용해 자동완성 기능을 구현했다는 것만으로도 2019년 소소한 목표 달성이라고 자축해봅니다. 새해에는 조금 더 욕심을 내어 1개로 끝냈던 일들을 2~3개 벌려보도록 노력해야겠습니다.

아 완성된 결과물은 아래엣 :-D

<p class="codepen" data-height="500" data-theme-id="light" data-default-tab="js,result" data-user="moonspam" data-slug-hash="WNNJRbd" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Vue.js Example - Autocomplete">
  <span>See the Pen <a href="https://codepen.io/moonspam/pen/WNNJRbd">
  Vue.js Example - Autocomplete</a> by moonspam (<a href="https://codepen.io/moonspam">@moonspam</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
