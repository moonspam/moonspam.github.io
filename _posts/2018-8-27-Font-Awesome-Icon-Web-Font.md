---
layout: post
title: Font Awesome 아이콘 세트 소개
tags: [Fonts, FontAwesome]
---

아이콘에 인터렉티브한 효과를 줄 경우 `SVG` 또는 `Base64`로 일일이 만들어 사용하는 경우가 있는데, 이 부분을 해결해줄 서비스가 있어 소개합니다.

## Font Awesome 소개

> 세계에서 가장 인기 있고 사용하기 쉬운 아이콘 세트

폰트어썸은 1,200여 개 무료 아이콘과 2,300여 개의 유료(Pro) 아이콘 세트로 구성되어 있으며, 따로 파일을 받을 필요 없이 CSS 파일을 불러오기만 하면 해당 아이콘 세트를 사용할 수 있습니다.

<i class="fab fa-fort-awesome-alt"></i> <https://fontawesome.com/>

## 폰트어썸 설정하기

`<head>` 안에 아래 종류 중 하나를 넣고 `<body>` 안에 `<i>` 또는 `<span>` 태그를 이용하여 아이콘을 불러올 수 있습니다.

### All

```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/all.css" integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ" crossorigin="anonymous">
```

### Solid

```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/solid.css" integrity="sha384-wnAC7ln+XN0UKdcPvJvtqIH3jOjs9pnKnq9qX68ImXvOGz2JuFoEiCjT8jyZQX2z" crossorigin="anonymous">
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/fontawesome.css" integrity="sha384-HbmWTHay9psM8qyzEKPc8odH4DsOuzdejtnr+OFtDmOcIVnhgReQ4GZBH7uwcjf6" crossorigin="anonymous">
```

### Regular(Pro)

```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/regular.css" integrity="sha384-zkhEzh7td0PG30vxQk1D9liRKeizzot4eqkJ8gB3/I+mZ1rjgQk+BSt2F6rT2c+I" crossorigin="anonymous">
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/fontawesome.css" integrity="sha384-HbmWTHay9psM8qyzEKPc8odH4DsOuzdejtnr+OFtDmOcIVnhgReQ4GZBH7uwcjf6" crossorigin="anonymous">
```

### Brands

```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/brands.css" integrity="sha384-nT8r1Kzllf71iZl81CdFzObMsaLOhqBU1JD2+XoAALbdtWaXDOlWOZTR4v1ktjPE" crossorigin="anonymous">
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.2.0/css/fontawesome.css" integrity="sha384-HbmWTHay9psM8qyzEKPc8odH4DsOuzdejtnr+OFtDmOcIVnhgReQ4GZBH7uwcjf6" crossorigin="anonymous">
```

## 폰트어썸 사용하기

[Solid](https://fontawesome.com/icons?s=solid)와 [Brands](https://fontawesome.com/icons?s=brands)는 **무료**로 사용할 수 있고, [Regular](https://fontawesome.com/icons?s=regular)와 [Light](https://fontawesome.com/icons?s=light)는 **Pro 버전 구매** 시 이용 가능합니다.

| 스타일  | 라이선스  | 접두사 | 예시                                            | 아이콘                              |
| ------- | --------- | ------ | ----------------------------------------------- | :---------------------------------: |
| Solid   | 무료      | fas    | &lt;i class="fas fa-stroopwafel"&gt;&lt;/i&gt;  | <i class="fas fa-stroopwafel"></i>  |
| Regular | 유료(Pro) | far    | &lt;i class="far fa-stroopwafel"&gt;&lt;/i&gt;  | Pro 구매 시 적용 가능               |
| Light   | 유료(Pro) | fal    | &lt;i class="fal fa-stroopwafel"&gt;&lt;/i&gt;  | Pro 구매 시 적용 가능               |
| Brands  | 무료      | fab    | &lt;i class="fab fa-font-awesome"&gt;&lt;/i&gt; | <i class="fab fa-font-awesome"></i> |

## 아이콘 검색하기

3,600여 개가 되는 아이콘을 일일이 찾기가 어렵기 때문에 [사이트](https://fontawesome.com/icons) 내 검색창에 키워드로 검색하여 쉽게 아이콘 코드를 알 수 있습니다.
유니코드도 제공하고 있어 [치트시트](https://fontawesome.com/cheatsheet)를 통해 한 번에 확인 가능합니다.