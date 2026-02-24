---
layout: post
title: NPM 패키지 편하게 관리하자(npm-check-updates 사용법)
tags: [Nodejs, NPM, Javascript]
---

NPM 패키지 업데이트할 때마다 뜨는 보안 경고 때문에 해당 패키지 GitHub 페이지로 들어가 새 업데이트가 없는지 정책이 바뀐 게 없는지 확인하는 것도 귀찮고 하나의 일이 돼버렸다.

특히 `npm update`는 다른 패키지의 종속성을 유지하면서 올리기 때문에 메이저 업데이트 같은 경우에 일일이 판올림 해줘야 하는 경우가 있다.

> 💡 업데이트가 필요한 패키지를 한눈에 보려면 `npm outdated`로 확인가능

그리하여 좀 더 편하게 패키지를 업데이트할 방법이 있는지 찾아보니 `npm-check-updates`라는 패키지를 알게 되어 간략하게 정리해본다.

## 설치

`npm-check-updates` 전역으로 설치

```bash
npm install -g npm-check-updates
```

## 사용 방법

업데이트가 필요한 프로젝트 경로로 가서 `ncu`라고 넣고 실행하면 아래와 같이 실행된다.

```console
$ ncu
Checking package.json
[====================] 5/5 100%

 eslint             7.32.0  →    8.0.0
 prettier           ^2.7.1  →   ^3.0.0
 svelte            ^3.48.0  →  ^3.51.0
 typescript         >3.0.0  →   >4.0.0
 untildify          <4.0.0  →   ^4.0.0
 webpack               4.x  →      5.x

Run ncu -u to upgrade package.json
```

> 💡 특정 패키지만 선택하여 업데이트 하려면 `ncu -i`를 이용  
> 💡 그룹으로 묶어 업데이트 하려면 `ncu -i --format group`를 이용

```console
$ ncu -i --format group
Upgrading package.json
[====================] 40/40 100%

? Choose which packages to update » 
  ↑/↓: Select a package
  Space: Toggle selection
  a: Toggle all
  Enter: Upgrade 

Patch   Backwards-compatible bug fixes
❯ (*) core-js                 ^3.26.0  →  ^3.26.1
  (*) css-loader               ^6.7.1  →   ^6.7.2
  (*) sass                    ^1.56.0  →  ^1.56.1
  (*) vue-template-compiler   ^2.7.13  →  ^2.7.14

Minor   Backwards-compatible features
  (*) sass-loader             ^13.1.0  →  ^13.2.0
  (*) webpack                 ^5.74.0  →  ^5.75.0

Major   Potentially breaking API changes
  ( ) axios                   ^0.27.2  →   ^1.1.3
  ( ) vue                     ^2.7.13  →  ^3.2.45
  ( ) vue-loader             ^15.10.0  →  ^17.0.1
  ( ) vue-router               ^3.6.5  →   ^4.1.6
```

해당 버전으로 업데이트를 진행하고 싶으면 `ncu -u`로 package.json 파일 수정

```console
$ ncu -u
Upgrading package.json
[====================] 5/5 100%

 eslint             7.32.0  →    8.0.0
 prettier           ^2.7.1  →   ^3.0.0
 svelte            ^3.48.0  →  ^3.51.0
 typescript         >3.0.0  →   >4.0.0
 untildify          <4.0.0  →   ^4.0.0
 webpack               4.x  →      5.x

Run npm install to install new versions.

$ npm install      # update installed packages and package-lock.json
```

`npm install`로 업데이트 설치를 진행하면 최신버전으로 업데이트가 반영됨.

전역으로 설치한 패키지는 `ncu -g`를 이용하여 업데이트 진행하면 된다.

### 예외 처리

vue.js 관련 패키지와 axios를 제외하고 업데이트를 진행할 경우 정규식을 사용하여 예외 처리할 수 있다.

```console
$ ncu "/^(?!vue|axios).*$/"
[====================] 5/5 100%

All dependencies match the latest package versions :)
```

> 💡 예외 처리된 패키지의 마이너 업데이트를 진행하려면 `ncu -i -t minor` 입력
