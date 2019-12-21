---
title: Nuxt] Middleware
date: 2019-10-29 20:11:53
tags: ['Nuxt', 'Middleware']
categories: ['Nuxt']
---

## Nuxt universal storage

<small> https://github.com/nuxt-community/universal-storage-module </small>

> Nuxt의 서버랜더링에서는 클라이언트 부분에 접근할 수 없다.
>
> middleware를 사용함에 있어서 LocalStorage와 같은 부분들에 접근할 수 없었고,
>
> window 객체를 사용할 수도 없었다. 또한, context.store에 접근하여도 초기값을 가져오고 이후 변경된 값을 가져온다 하여도 F5로 새로고침 되거나 다른 페이지로 넘어가는 순간 다시 초기값이 되었다.
>
> 이를 해결하기 위해 Nuxt에서 LocalStorage를 사용할 수 있는 모듈을 설치하였다.



nuxt mode에서는 **spa**와 **universal**이 있다.

1. spa

   > 서버측 렌더링 없음(클라이언트 탐색만)

2. universal

   > 동적 응용 프로그램(서버측 랜더링 + 클라이언트 탐색)

<small>사이트:  https://nuxtjs.org/api/configuration-mode#the-mode-property </small>


## 느낀점

nuxt에 `process.browser`와 `process.server`가 있는 것으로 보아 브라우저일 때와 서버일 때가 나눠져 있다. 더 재미있는 사실은 두 개가 다 아닐 때도 있다. 이 설정이 nuxt.config.js에 env에 있는 것 같다.(정확히 파악은 하지 않음)

Middleware를 어떻게 쓸 것인가가 가장 큰 관건인데, 서버단에서 context를 이용하지 않은 store접근은 불가하다고 보는게 낫다. 그리고 기본적으로는 window도 사용할 수 없다.

redirect시에 store가 날라가는 위험성도 존재하니, 이를 보완하기 위해 storage나 cookie에 일부 데이터를 저장하고 비동기 통신을 통해서 가져오는 것이 어떠한가 생각한다.

현재로서의 결론] user의 id나 email정도를 저장소에 보관하고 axios를 통해 DB값을 가져와서 악의적 사용자가 발생하지 않도록 예방하기