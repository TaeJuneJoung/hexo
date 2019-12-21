---
title: Django] JWT Token
date: 2019-12-03 23:21:45
tags: ['JWT', 'Django']
categories: ['Django']
---


## JWT를 왜 사용해야하는가?
Json Web Token
: 전자 서명 된 URL-safe(URL로 이용할 수 있는 문자만 구성된)의 JSON


## Settings

실행한 프로젝트 구조 URL
https://github.com/TaeJuneJoung/Python-Deploy/tree/jwt-token

### install

```bash
pip install djangorestframework djangorestframework-jwt
```

### Django Settings

아래의 소스를 Settings의 가장 아래 부분에 추가

```python
import datetime

REST_FRAMEWORK = {
    # 로그인 여부를 확인하는 클래스
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    # 로그인과 관련된 클래스
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ),
}

JWT_AUTH = {
    'JWT_SECRET_KEY': SECRET_KEY,
    'JWT_ALGORITHM': 'HS256',
    'JWT_ALLOW_REFRESH': True,
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=7),
    'JWT_REFRESH_EXPIRATION_DELTA': datetime.timedelta(days=28),
}
```

**- JWT_SECRET_KEY**

> JWT의 비밀키로 어떤걸 사용할지 작성
>
> 여기서는 Django의 비밀키를 사용하였으나, **다른키 사용 권장**

**- JWT_ALGORITHM**

> JWT 암호화에 사용되는 알고리즘을 지정

**- JWT_ALLOW_REFRESH**

> JWT 토큰을 갱신할 수 있게 할지 여부를 결정

**- JWT_EXPIRATION_DELTA**

> JWT 토큰의 유효 기간 설정
>
> 위의 설정과 같은 경우는 JWT 토큰을 7일 안에 갱신하지 않으면 JWT토큰을 사용할 수 없고 로그아웃 된다.

**- JWT_REFRESH_EXPIRATION_DELTA**

> JWT 토큰 갱신의 유효기간
>
> 위의 설정과 같은 경우 7일 안에 갱신하여도 28일 후에는 갱신할 수 없다. 즉, 28일 후에는 로그아웃 된다.







### urls.py

```python
from django.contrib import admin
from django.urls import path, include
from rest_framework_jwt.views import obtain_jwt_token, verify_jwt_token, refresh_jwt_token

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/token/', obtain_jwt_token), #JWT토큰 발행
    path('api/token/verify/', verify_jwt_token), #JWT유효 검증
    path('api/token/refresh/', refresh_jwt_token), #JWT토큰 갱신
    path('', include('todos.urls')),
]
```



### todos.views.py

```python
from django.shortcuts import render
from django.core import serializers
from django.http import HttpResponse
from rest_framework.decorators import api_view, permission_classes, authentication_classes
from rest_framework.permissions import IsAuthenticated #로그인 여부 확인
from rest_framework_jwt.authentication import JSONWebTokenAuthentication #JWT 인증을 확인하기 위해 사용

from .models import Todos

@api_view(['GET'])
@permission_classes((IsAuthenticated,)) #권한 체크(현재 여기서는 로그인 여부만 체크)
@authentication_classes((JSONWebTokenAuthentication,)) #JWT토큰 확인(토큰 이상시 JSON으로 에러 반환)
def main(request):
    todos = Todos.objects.all()
    todos_list = serializers.serialize('json', todos)

    return HttpResponse(todos_list, content_type='text/json-comment-filtered')
```





## PostMan을 통한 확인

### JWT TOKEN 생성

```
[POST]
http://127.0.0.1:8000/api/token/

Body -> form-data
username:
password:
```

**결과값**

```json
{
    "token": "토큰값"
}
```

실패시

```json
{
    "non_field_errors": [
        "Unable to log in with provided credentials."
    ]
}
```



**데이터 값 확인**

```
[GET]
http://127.0.0.1:8000

Headers
Authorization: jwt 토큰값
```

**결과값**

```json
[
    {
        "model": "todos.todos",
        "pk": 1,
        "fields": {
            "content": "확인!"
        }
    }
]
```



### JWT TOKEN 검증

```
[POST]
http://127.0.0.1:8000/api/token/verify/

Body -> form-data
token: 토큰값
```

**결과값**

```json
{
    "token": "토큰값"
}
```

실패시

```json
{
    "non_field_errors": [
        "Error decoding signature."
    ]
}
```



### JWT TOKEN 갱신

```
[POST]
http://127.0.0.1:8000/api/token/refresh/

Body -> form-data
token: 토큰값
```

**결과값**

```json
{
    "token": "변경된 토큰값"
}
```

실패시

```json
{
    "non_field_errors": [
        "Error decoding signature."
    ]
}
```






## 참고자료
 http://www.opennaru.com/opennaru-blog/jwt-json-web-token/
 https://brownbears.tistory.com/440
 https://dev-yakuza.github.io/ko/django/jwt/ 