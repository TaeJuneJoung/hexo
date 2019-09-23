---
title: PWA
date: 2019-07-27 23:45:12
tags: ['PWA']
categories: ['PWA']
---

# PWA

**Progressive Web App**

웹과 네이티브 앱의 기능 모두의 이점을 갖도록 수 많은 특정 기술과 표준 패턴을 사용해 개발된 웹 앱.



### Intall Setting

편리한 확인을 위해 `Chrome canary(개발자 크롬)`를 다운받아 진행.

`Node.js` Download



### PWA의 등장 배경

- 모바일 시장 폭발적 성장
- 모바일 웹 보다는 모바일 애플리케이션 사용
- 모바일 앱영역 커버하기 위한 시도 -> Hybrid App, React Native
- Offline Web 필요성



### 특징

1. Responsive : 반응형
2. App-like
3. Discoverable
4. Engageable : 푸시 알림
5. Connectivity : 온/오프라인 연결성
6. Safe : HTTPS가 아니라면 PWA를 적용할 수 없음_보안을 위해서



### manifest.json

**Web App Manifest**

 : Progressive Web App의 설치와 앱 구성정보를 담고 있는 json 형식의 설정 파일

	- 앱 아이콘, 화면 런처 방식, 배경색, 시작 페이지 등을 설정할 수 있는 JSON 파일

> 앱 관련 구성정보
>
> 1. Start URL : 웹 앱이 시작되는 지점
> 2. Launch Image : 웹 앱 시작 화면
> 3. Display Type : 웹 앱의 화면 형태
> 4. Display Orientation : 웹 앱 화면 방향
> 5. App Icon : 앱 아이콘 이미지와 크기



```json
/*Sample*/
{
 "short_name": "앱 아이콘 이름",
 "name": "하단 설치 배너 표기될 이름 / 앱 검색 키워드",
 "icons": [
  {
   "src": "\/android-icon-36x36.png",
   "sizes": "36x36",
   "type": "image\/png",
   "density": "0.75"
  },
  {
   "src": "\/android-icon-96x96.png",
   "sizes": "96x96",
   "type": "image\/png",
   "density": "2.0"
  },
  {
   "src": "\/android-icon-192x192.png",
   "sizes": "192x192",
   "type": "image\/png",
   "density": "4.0"
  }
 ],
 "background_color": "#ffffff",
 "display": "standalone",
 "start_url": "./"
}
```

```html
<!-- manifest파일 등록 -->
<link rel="manifest" href="Path/manifest.json">
```



**Launch Image**

App Web이 시작될 때 거치는 시작 화면 설정 가능

모바일 앱의 시작과 동일한 느낌

화면 조합 : 아이콘 + 배경색 + 아이콘 이름

```json
"background_color": "#ffffff",
```

icon은 지정 이미지 중 128dp(192px)에 가장 가까운 크기로 지정되므로, 192px 이미지는 꼭 넣어줄 것

> **dp**
>
> 다양한 모바일 화면 크기에서 동일한 비율로 출력되게 하는 픽셀 단위



**App Icon**

manifest에서 icon 미지정시 html파일의 `<link rel="icon">` 태그를 검색

> **브라우저 Safari 경우** 아래 추가 필요
>
> ```html
> <link rel="apple-touch-icon" href="icon.png">
> <link rel="apple-touch-icon" sizes="152x152" href="icon-152x152.png">
> <link rel="apple-touch-icon" sizes="180x180" href="icon-180x180.png">
> <link rel="apple-touch-icon" sizes="167x167" href="icon-167x167.png">
> ```
