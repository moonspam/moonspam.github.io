---
layout: post
title: 나눔 고딕 웹 폰트에 대한 고찰
tags: [NanumGothic, Fonts]
---

굴림과 돋움의 고인물이 될 무렵 나눔 고딕이 세상에 나오고 한글 웹 폰트의 부흥이 시작되었다는...  
(뇌피셜) 구글 폰트 [Early Access](https://fonts.google.com/earlyaccess) 사이트에서 나눔 고딕이 올라오자마자 여러 사이트에서 나눔 고딕을 사용하였고, 2018년부터 정식으로 구글 폰트에서 이용할 수 있습니다.

<https://fonts.google.com/specimen/Nanum+Gothic>

## 웹 폰트에 대한 고민

한글 웹 폰트의 단점은 용량이 크다는 점인데, 이 부분을 해결하려고 여러 방법을 고민했었습니다. 필요한 글자만 추출해서 사용하는 경량화(subset) 작업이나 웹 폰트에서 경량화된 폰트를 제공해주는 케이스가 있었습니다. (예: 나눔 스퀘어) 나눔 스퀘어같이 경량화된 웹 폰트를 제공해주는 케이스가 많이 없었기 때문에, 서브셋 폰트를 한번 만들어 놓으면 사골 끓이듯 쓰고 또 썼습니다.

다행히 구글에서 머신 러닝에 기반으로 패턴에 따라 한글 폰트를 나눠서 불러오는 방식을 도입하여 위 문제들을 어느 정도 해소해주고 있습니다. `unicode-range`를 이용하여 폰트를 여러 개로 쪼갤 생각을 했다니 구글스 칭찬해! 👍  
그리하여 어마어마한 용량을 자랑하는 `Noto Sans KR`도 부담 없이 사용 중입니다.

## 나눔... 나눔 고딕을 보자

나눔 고딕이 PPT, 이미지, 영상 등등 국내에서 널리 사용되고 있어 ttf 파일을 이용하는 분들도 많을 거라 생각이 듭니다...(뇌피셜) 그리하여 나눔 고딕을 웹 폰트를 제공하지만 PC에 나눔 고딕이 있다면 받지 않도록 CSS 스타일 선언하는 방법 등 여러 가지 고민해보고 찾아본 내용을 나열해 보겠습니다.

## 결과부터

CSS `font-family` 속성에서 PC에 설치되어 있는 나눔 고딕을 먼저 선언해놓으면 뒤에 선언되어있는 웹 폰트를 브라우저에서 받지 않습니다. 하지만 OS 별 브라우저마다 인식하는 나눔 고딕 이름의 차이가 있기 때문에 한글과 영문을 동시에 선언하여 처리해주면 깔끔하게 해결됩니다.

|   | IE | Firefox | Chrome | Safari | Opera | Safari(Mac) | Firefox(Mac) | Chrome(Mac) |
|---|---|---|---|---|---|---|---|---|
| 나눔고딕 | O | O | O | O | O | X | O | X |
| NanumGothic | O | O | X | O | X | O | O | O |

> 출처: <http://www.beautifulcss.com/archives/431>

### HTML

`font-display: swap`은 [지원하는 브라우저](https://caniuse.com/#feat=css-font-rendering-controls)가 제한적이니 `swap` 방식으로 처리하실 땐 [webfontloader](https://github.com/typekit/webfontloader)를 이용해보시는 걸 추천합니다.

```html
<link href="https://fonts.googleapis.com/css?family=Nanum+Gothic&display=swap" rel="stylesheet">
```

### CSS

```css
body {
  font-family: 나눔고딕, 'NanumGothic', Nanum Gothic, sans-serif;
}
```

## 굵기가 달라

<figure class="video">
  <iframe src="https://giphy.com/embed/13xNBYYT0fxAs0" frameborder="0" allowfullscreen="true"></iframe>
</figure>

위 방법으로 나눔 고딕을 적용하다 보면 PC에 설치한 나눔 고딕의 `bold`와 구글 웹 폰트의 `bold`의 굵기 차이가 발생합니다. 완벽하진 않지만 해당 부분은 아래 스타일 선언으로 해결할 수 있습니다.

```css
strong {
  font-weight: 600;
}
```

<p class="codepen" data-height="350" data-theme-id="light" data-default-tab="result" data-user="moonspam" data-slug-hash="YzKedPp" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="NanumGothic Test">
  <span>See the Pen <a href="https://codepen.io/moonspam/pen/YzKedPp/">
  NanumGothic Test</a> by moonspam (<a href="https://codepen.io/moonspam">@moonspam</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 참고문서

한글 웹 폰트를 사용하기에 참고하면 좋을 링크들을 공유하면서 포스팅을 마치도록 하겠습니다.

- <https://googlefonts.github.io/korean/>
- <http://youngkang.me/hangul-webfont-showcase/>
- <https://lqez.github.io/blog/hangul-typo-on-web.html>
- <https://slowalk.tistory.com/2194>
- <https://sangziii.github.io/fontStyleMatcher/>
