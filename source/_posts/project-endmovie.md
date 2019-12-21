---
title: Project] End Movie
date: 2019-11-14 22:30:59
tags: ['Project']
categories: ['Project']
---

## 프로젝트 목적

이전에 Django를 처음 배우면서 만들었던 영화 프로젝트를 `RESTFUL`을 이용한 방법으로 만들어보고자 토이 프로젝트를 진행하였다.
1. RESTFUL 학습
2. Front-END 학습

초기 프로젝트 주소
: https://github.com/ClearRoot/EndGamePJ

현재 프로젝트 주소
: https://github.com/TaeJuneJoung/endMovie


{% asset_img slug guest_main.png %}

The Movie DB API를 이용하여 데이터를 가져와 DB에 저장한 후, 처음 페이지에서 보여주었다.
너무 많은 데이터를 한꺼번에 보여줘서 스크롤의 압박이 생길까하여 처음에는 28개의 영화 정보를 보여주고 `더보기` 버튼을 누르면 12개씩 추가되도록 하였다.

{% asset_img slug guest_movie_detail01.png %}

하나의 영화 정보를 클릭해서 들어가면 처음 맞이하는 페이지

{% asset_img slug guest_movie_detail02.png %}

아래의 공간에는 Youtube를 통해 예고편 보기와 영화 내용, 댓글 기능이 구현되어 있다.



{% asset_img slug join.png %}

회원은 회원이름을 ID로 하고, 비밀번호는 숫자+영어+특수문자로 8자 이상을 기입하여야 가능하다. 이메일 인증을 통해 무분별한 사용자를 막고자 하였다.(휴대전화로 하려고 하였으나, 문자는 금액적인 부분이 발생으로 이메일로 구현)

{% asset_img slug login.png %}

회원가입과 로그인 디자인 부분은 `N`사에서 얻어왔다.

{% asset_img slug user_main.png %}

로그인을 한 사용자는 영화의 평점도 영화 리스트를 보며 남길 수 있다.

{% asset_img slug user_movie_detail01.png %}

로그인 사용자는 댓글을 작성하며 댓글에 대한 `좋아요` 기능을 사용할 수 있다.


## 추후 발전 계획 내용

1. 영화 추천 서비스
2. 배포(Deploy)
3. 검색기능
4. 좋아요 기능
5. 댓글 신고 기능 / BEST 댓글
6. 약관동의/회원 분실 찾기

1번과 2번은 추후라도 꼭 해볼 내용
1번은 AI부분이나 빅데이터 부분을 배워야하기에 학습을 이룬 후에 1번과 2번을 위주로 할 계획

3~6번은 서비스에는 필요하나 학습 위주의 토이 프로젝트이기에 구현이 어렵지 않으므로 넘길 수 있음

2번의 배포는 Docker를 이용하고 CI/CD 자동화를 한 후에 가능하다면 `kubernetes`를 알아보고 적용해볼 계획


## 프로젝트를 하며 느낀점

현재까지 작성한 부분은 `영화 추천 서비스`를 하기 위해 기본적인 부분들을 만든 토이 프로젝트다.
다른 부분들을 하면서 시간이 날 때마다 조금씩 하다보니 집중력이 많이 들어간 프로젝트는 아닌 점이 아쉽다.
그래도 Front를 Vue와 Vuetify로 만들면서 정말 편하다는 점과 Back단만 고집했던 나 자신이 Full Stack으로 하면서 '하나의 기본적인 프로젝트를 완성할 수 있구나'라는 자신감을 느낄수 있었다.
서버단에서 RESTFUL을 이용할 때는 Django는 Serailizer를 이용해야하니 Django에서 바로 서비스를 만들때랑은 다르겠구나 하였는데, 기본은 어디에서나 쓰이고 기본이 튼튼해야함을 느낄 수 있었다.

### 다른 프로젝트에서 적용해볼 내용

- JWT Token
현 프로젝트에서는 회원 정보를 JWT토큰으로 주고 받고 있지 않기에 JWT토큰에 대한 부분을 만들어 봐야겠다.

- TDD 개발
TDD개발을 어떻게 하는지 감은 오는데 정확한 방법을 몰라 연습해 봐야겠다. 감 잡은 내용은 아래의 주소와 같다.

https://wikidocs.net/11057

