---
layout: post
title: 구글 크롬 자동재생 미디어 정책 변경 내용 공유 
tags: [API, Chrome, YouTube]
---

2018년 4월부터 크롬 브라우저에서 페이지 로딩 시 영상 자동재생(autoplay)이 못하도록 정책이 변경되었습니다. 갑자기 나오는 영상 때문에 심장 어택을 방지하려고 추가된 정책 같습니다.

![I will find you](https://i.imgflip.com/ngd6c.jpg)

아래의 경우 일부 자동 재생을 허용한다고 하니 작업 시 참고하시면 될 것 같습니다.

- 음소거 된 자동 재생은 항상 허용
- 사용자가 상호 작용했을 시(클릭, 탭 등)
- 데스크톱에서 [Media Engagement Index (MEI)](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes#mei) 를 만족할 경우
- 모바일에서는 사용자가 [자신의 홈 화면에 사이트를 추가](https://developers.google.com/web/fundamentals/integration/webapks)할 경우

위 정책에 맞게 영상 BG 작업 시 HTML5 video 태그나 YouTube iframe에 음소거 하는 방법을 공유합니다.

## HTML `video` Tag

`video` 태그 내 muted 속성 추가

```html
<video muted autoplay loop>
```

## YouTube `iframe` Tag

`iframe` `src` 속성에 `&amp;mute=1` 추가

```html
<iframe src="https://www.youtube.com/embed/YOUTUBEID?rel=0&amp;autoplay=1&amp;mute=1" width="560" height="315" frameborder="0" allowfullscreen></iframe>
```

출처: <https://developers.google.com/web/updates/2017/09/autoplay-policy-changes>