---
title: AWS Lambda
date: 2019-07-22 23:19:01
tags: ['AWS Lambda']
categories: ['AWS']
---

# AWS Lambda

`aws lambda`에서 함수 생성

새로 작성 -> 기본정보

> 함수 이름:
>
> 런타임: 본인 사용 언어
>
> 권한: 실행 역할 -> 기존 역할 사용 or 새 역할



```python
#해당 내용에서는 Slack을 이용하여 오전 10시마다 정보를 전달해주는 함수를 생성
#기본적으로 Amazon CloudWatch Logs가 있기에
CloudWatch Events를 만들어줘야 한다.
'CloudWatch Events'를 이용하여 평일 10시마다 이벤트 발생
일정을 'cron(0 1 ? * MON-FRI *)'으로 하여 평일마다 진행하게 하여주고
기본적으로 GMT시간을 따르는데 한국시간과 9시간 차이나므로(GMT가 9시간 전)
한국시간으로 10시를 해주기 위해서는 -9를 해준 1이 됨
```



API Gateway를 이용해야할 때도 있으나, 현재 하는 부분에 대해서는 API Gateway가 불필요하여 사용해보지 않았음.



### Slack 설정

`Customize Slack`에서 설정

해당 workspace에 대한 권한이 있어야함을 기본으로 한다.

`Configure Apps`에서 IFTTT를 만들고, 해당 aws연결할 app을 만들어 진행하면 된다.



### 추후 해볼 내용

만든 프로젝트에서 실시간 데이터를 가져와줘야하는 부분 함수가 있다면 해당 함수만 시간에 따라 진행하도록 하는 방법

> **example**
>
> 영화 프로젝트에서 데이터를 가져오는 파싱 부분을 새벽00시를 기준으로 실행하여 데이터를 변경하는 방법 구현