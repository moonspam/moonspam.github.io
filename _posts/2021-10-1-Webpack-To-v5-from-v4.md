---
layout: post
title: 웹팩(Webpack) v4에서 v5로 마이그레이션
tag: [Webpack, Migration]
---

Webpack v4를 잘 사용하고 있었는데, `npm update`나 `install` 할 때마다 플러그인들이 지원을 종료했다고 메시지가 나오고 보안 관련 부분에서 `high` 이상의 문제가 자꾸 발생하여 원만한 유지보수를 위해 v5로 마이그레이션 작업을 진행하였다.

![Imgur](https://i.imgur.com/LoZpqLf.png)
*눈에 거슬리는 deprecated...*

## 업데이트하자

보안 경고 때문에 `npm audit fix --force`를 통해 강제로 v5로 업데이트를 하게되면 플러그인들이 꼬여서 구동이 안 되는 현상을 발견하여, 최초 설정(`npm init`)부터 차근차근 진행하였다.

작업할 때 사용하고 있는 웹팩 템플릿(최하단 링크 참고) 기준으로 v4에서 v5로 버전업할 때 주요 변경점은 아래와 같았다.

### package.json

- `eslint-loader` *deprecated* => `eslint-webpack-plugin` 교체
- `@babel/polyfill` *deprecated* => `@babel/plugin-transform-runtime` 교체(전역 오염 방지)
- `webpack-dev-server` *3.11.0* => *4.3.0* 업데이트
- `--env.NODE_ENV=production` => `--mode=production` 변경

Webpack v5 대응을 위해 `webpack-dev-server` 플러그인 같은 경우에는 강제로 *4.3.0* 버전으로 설치해야 한다. beta 버전일 땐 삭제되거나 추가되는 사양이 계속 바뀌어서 관련 문서를 업데이트 때마다 읽고 작업해야 했다.

`@babel/polyfill` 같은 경우에도 구익스(ie11) 외 최신 브라우저에서도 `polyfill`이 반영이 되었는데, `@babel/plugin-transform-runtime` 교체하면서 구익스에서만 `polyfill` 처리가 되고 최신 브라우저에서는 구동되지 않게 설정하였다. 이 부분이 마이그레이션 작업하면서 제일 오래 걸렸다.

#### 참고 블로그

- <https://tech.kakao.com/2020/12/01/frontend-growth-02/>
- <https://okchangwon.tistory.com/3?category=676183>

### webpack.config.js

- `module.exports = (env) => {...` => `module.exports = (env, argv) => {...` 변경
- `env.NODE_ENV === 'development'` => `argv.mode === 'development'` 변경

기존에는 `env.NODE_ENV` 환경변수로 개발 모드와 프로덕션 모드를 구분하였는데, 해당 부분이 삭제되면서 `argv.mode`를 이용하여 구분하도록 변경되었다.

그 외 플러그인들은 각각의 가이드 문서를 참고하면서 조금씩 변경이 이루어졌다.

마이그레이션을 진행하면서 기존에 유용하게 사용했던 부분이 삭제/변경된 부분이 있었는데, html 안에 html을 include 해주던 기능을 사용할 수 없게 된 부분이 타격이 컸다. 해당 이슈에 불편해하는 사용자들을 깃헙에서 볼 수 있는데, 이 부분은 [개발 진행 중][link1]이다.

그리고 `${require('이미지경로')}` 이렇게 템플릿 리터럴(Template literals)로 표현식을 처리했던 부분을 `<%=require('이미지경로')%>` 이 형식으로 변경을 해야 했던 부분도 있었다. (생각보다 시간을 많이 소요했다;;)

## 마무리

다이나믹하게 업그레이드되었다고 느낄 순 없었지만 이번 작업을 통해서 전역 오염 없는 폴리필 설정도 해보고 자주 업데이트하며 유지 보수해야 했던 일들이 줄어들었다는 점에서 만족감을 느끼고 있다.

내가 사용하고 있는 Webpack 템플릿을 공유하며 마무리하겠다.

[webpack-base-template][link2]

[link1]: <https://github.com/webpack-contrib/html-loader/issues/291>
[link2]: <https://github.com/moonspam/webpack-base-template>
