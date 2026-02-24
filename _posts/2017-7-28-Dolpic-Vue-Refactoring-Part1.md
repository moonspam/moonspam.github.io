---
layout: post
title: Vue 입문기 - 돌픽 Vue.js로 리팩토링하기 1
tags: [Vuejs, Dolpic, Javascript]
---

빠르게 변화하는 것을 따라잡으려면 부지런해야 한다는 것을 요즘 들어 많이 느끼고 있습니다. 스스로 자극을 주기 위해 취업 사이트를 둘러보거나 기술 관련 글들을 보지만, 실무에 새로운 것을 도입한다는 것이 여러모로 힘든 부분이 많습니다. 그리하여 실력 증진을 위한 스스로 프로젝트를 결심하게 되었습니다.

최근 저의 학구열을 불태우게 해준 글모음입니다.

- [웹 프론트엔드 개발자, 어떻게 준비해야 할까?](https://medium.com/@codesquad_yoda/%EC%9B%B9-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A4%80%EB%B9%84%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C-5ac7bb6ff2a9)
- [탁월한 프론트엔드 엔지니어가 되는 법](https://hyunseob.github.io/2016/02/21/how-to-become-a-great-frontend-engineer/)
- [프론트엔드 개발자는 왜 구하기 어렵나요?](https://taegon.kim/archives/4810)

<center><img src="https://kr.vuejs.org/images/logo.png" width="250" height="250"></center>

## 삽을 들다

어디서부터 어떻게 시작해야 할까 고민하는 중에 친한 개발자분과 소소하게 만들었던(그때는 야심 차게?) [Dolpic](http://dolpic.kr/)이란 사이트를 이용해보기로 했습니다.

React.js와 Vue.js 중에 고민하는 찰나 친절하게 한글로 되어있는 Vue.js로 삽을 들었습니다.

<https://kr.vuejs.org/>

## 삽질을 하다

Vue 가이드 문서는 한글인데 이해가 안 가는 부분이 많았습니다. 작업하면서 이해를 해보자는 생각으로 코드를 적기 시작했습니다.

### vue-cli 설치

공식 가이드에서는 Vue.js를 빠르게 스캐폴딩(?) 하기 위해 CLI를 제공한다고 적혀있습니다.  
vue-cli 설치 후 프로젝트를 만들어 보았습니다.

> npm을 사용하기 위해 Node.js가 설치되어 있어야 합니다.

*[스캐폴딩]: 문제를 정의할 때 학습자가 무엇을 고려해야 하는가에 대한 안내를 제공
*[npm]: Node Package Manger

```bash
# vue-cli 설치
$ npm install --global vue-cli
# "webpack" 템플릿을 이용해서 새 프로젝트 생성
$ vue init webpack dolpic-vue
# 프로젝트 폴더 이동
$ cd dolpic-vue
# 프로젝트 npm 설치
$ npm install
# localhost:8080 src/main.js 실행
$ npm run dev
```

> Vue CLI에서 다양한 옵션이 지원되는데 이 부분은 `vue list`로 확인 할 수 있습니다.

### 메인 페이지 구성

메인 페이지에서 API를 받아서 이미지를 보여주는 부분이 첫 번째 목표였습니다. 사이드와 하단 영역을 **컴포넌트로 분리**하고 **[Axios](https://github.com/mzabriskie/axios) 모듈을 이용하여 API를 받는 구조**로 그림을 그렸습니다.

현재까지 작업한 저장소도 공유합니다.  
<https://github.com/moonspam/dolpic-vuejs>

#### - src/main.js

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import axios from 'axios'
require('./assets/css/common.css')

Vue.config.productionTip = false
Vue.prototype.$http = axios

new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})
```

#### - src/App.vue

```html
<template>
  <div id="app">
    ...
    <div class="wrapper">
      <router-view></router-view>
      <dolpic-side></dolpic-side>
      <div class="clear"></div>
    </div>
    <dolpic-list></dolpic-list>
  </div>
</template>

<script>
import DolpicSide from './components/DolpicSide.vue'
import DolpicList from './components/DolpicList.vue'
export default {
  name: 'app',
  components: {
    'dolpic-side': DolpicSide,
    'dolpic-list': DolpicList
  }
}
</script>
```

`<router-view>` 라우터에 대해서는 첫 번째 숙제를 끝난 후 훑어보려 합니다. 최상위 경로에서 components/Main.vue 컴포넌트를 불러오고 해당 페이지에서 API를 불러오도록 하였습니다.

#### - src/components/Main.vue

```javascript
...

<script>
export default {
  name: 'main',
  data () {
    return {
      active: false,
      posts: []
    }
  },
  created () {
    this.listNew()
  },
  methods: {
    listNew () {
      const apiURL = 'http://dolpic.kr/api/listNew'
      this.$http.post(apiURL).then((result) => {
        console.log(result)
        this.posts = result.data
      })
    }
  }
}
</script>
```

Axios 모듈을 이용해 POST 방식으로 데이터를 받아와 posts에 저장하는 부분부터 저에게는 진입장벽이 높았습니다. 구글링해보니 Vue 공식 사이트에 [Axios를 사용한 예](https://vuejs.org/v2/cookbook/adding-instance-properties.html#Real-World-Example-Replacing-Vue-Resource-with-Axios)가 있어 참고하였습니다. Vue 인스턴스가 생성되었을 때 메소드에 있는 `listNew()` 함수를 호출하려면 [**라이프사이클 훅**](https://kr.vuejs.org/v2/guide/instance.html#인스턴스-라이프사이클-훅)을 이용하면 되었습니다. `created`, `mounted`, `updated` 등 몇 가지가 있는데 라이프사이클 다이어그램을 보면서 어느 시점에 호출이 되는지 아주 조금 이해할 수 있었습니다. (아주 조금...)

<center><img src="https://kr.vuejs.org/images/lifecycle.png" width="600"></center>

posts에 담긴 데이터를 반복문을 사용하여 뿌려줘야 하는데, 이 부분은 얕은 지식으로 어렵지 않게 구현할 수 있었습니다.

조건문과 반복문  
<https://kr.vuejs.org/v2/guide/#조건문과-반복문>

반복문 안에 `urlType` 값에 따라 클래스 이름이 변경되는 부분이 있습니다. 이 부분은 `v-bind:class`를 이용하여 전달받는 값에 따라 클래스 이름을 지정할 수 있었습니다.  배경 이미지 주소를 `style`에 넣는 부분도 `v-bind:style`로 사용해야만 노출이 잘되었습니다.

#### - src/components/Main.vue

```html
<template>
  <div class='gbox_gallery'>
  <!-- 생략 -->
    <div v-for='post in posts' :key='post._id'>
      <div class='gbox' v-bind:style="{ backgroundImage: 'url(' + post.url + ')' }">
        <div class='gbox_thumb' :class="[{g_t_twitter: post.urlType === 1}, {g_t_instagram: post.urlType === 2}, {g_t_facebook: post.urlType === 3}]">
          <div class='gbox_t_title'>{{post.hashTagId.twitterHashTag}}</div>
          <div class='gbox_t_text'>
            <img src='../assets/img/icon_like.png' height='9' alt='like'>{{post.likeCount}}
          </div>
        </div>
      </div>
    </div>
  <!-- 생략 -->
  </div>
</template>

...
```

참고 : <https://alligator.io/vuejs/dynamic-styles/>

## 삽질은 계속된다

내가 적은 코드가 제대로 구현되었을 때의 희열은 잊을 수 없습니다. 누구에게는 쉽고 기초적인 과정일 수 있지만, 나에게는 하나하나가 새롭고 재밌습니다.
~~다음번에는 나머지 API들을 호출하여 적용하는 작업을 할 예정입니다.  
다음 이 시간에!~~

<figure class="video">
  <iframe src="https://www.youtube.com/embed/b22Ed7f0D-0?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen="true"></iframe>
</figure>
> Apink(에이핑크) FIVE
