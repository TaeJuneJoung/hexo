---
title: oauth2
date: 2019-06-23 22:16:35
tags: ['oauth2', 'django']
categories: ['django', 'oauth2']
---

# oAuth2

### 참고사이트

<https://django-allauth.readthedocs.io/en/latest/>



### Install & Settings

**Install django-allauth**

```bash
pip install django-allauth
```

 

**settings.py**

 ```python
 # Specify the context processors as follows:
 TEMPLATES = [
     {
         'BACKEND': 'django.template.backends.django.DjangoTemplates',
         'DIRS': [],
         'APP_DIRS': True,
         'OPTIONS': {
             'context_processors': [
                 # Already defined Django-related contexts here
 
                 # `allauth` needs this from django
                 'django.template.context_processors.request',
             ],
         },
     },
 ]
 
 AUTHENTICATION_BACKENDS = (
     ...
     # Needed to login by username in Django admin, regardless of `allauth`
     'django.contrib.auth.backends.ModelBackend',
 
     # `allauth` specific authentication methods, such as login by e-mail
     'allauth.account.auth_backends.AuthenticationBackend',
     ...
 )
 
 INSTALLED_APPS = (
     ...
     # The following apps are required:
     'django.contrib.auth',
     'django.contrib.messages',
     'django.contrib.sites',
 
     'allauth',
     'allauth.account',
     'allauth.socialaccount',
     # ... include the providers you want to enable:
     'allauth.socialaccount.providers.kakao',
 )
 
 SITE_ID = 1
 
 LOGIN_REDIRECT_URL = "accounts:main"
 ```

 필자는 django 2.2.1버전으로 진행하였고,

 `AUTHENTICATION_BACKENDS`부터 추가하면 되었다.

 

 **urls.py**

 ```python
 from django.contrib import admin
 from django.urls import path, include
 
 urlpatterns = [
     path('admin/', admin.site.urls),
     path('accounts/', include('accounts.urls')),
     #allauth
     path('accounts/', include('allauth.urls')),
 ]
 ```

 

 **accounts/views.py**

 ```python
 from django.shortcuts import render, redirect
 from django.contrib.auth.forms import AuthenticationForm
 from django.contrib.auth import authenticate
 from django.contrib.auth import login as auth_login, logout as auth_logout
 
 # Create your views here.
 def main(request):
     return render(request, 'accounts/main.html')
 
 def login(request):
     if request.method == "POST":
         auth_id = request.POST.get('auth_id')
         auth_pwd = request.POST.get('auth_pwd')
         user = authenticate(request, username=auth_id, password=auth_pwd)
         if user is not None:
             auth_login(request, user)
             return redirect('accounts:main')
     else:
         form = AuthenticationForm()
     return render(request, 'accounts/login.html', {'form':form})
 
 def logout(request):
     auth_logout(request)
     return redirect('accounts:main')
 ```

 

 **main.html**

 ```html
 {% load socialaccount %}
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <meta http-equiv="X-UA-Compatible" content="ie=edge">
     <title>AllAuth</title>
 </head>
 <body>
     <div>
         <a href="{% url 'accounts:main' %}">Home</a>
         {% if user.is_authenticated %}
             <a href="{% url 'accounts:logout' %}">Logout</a>
         {% else %}
             <a href="{% url 'accounts:login' %}">Login</a>
         {% endif %}
     </div>
 
     <h1>Main</h1>
     {% if user.is_authenticated %}
         <p>{{request.user}}님, 로그인하셨습니다.</p>
     {% else %}
         <p>학습 목적이기에 꾸며지지 않았습니다.</p>
     {% endif %}
 </body>
 </html>
 ```

 

 **login.html**

 ```html
 {% load socialaccount %}
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <meta http-equiv="X-UA-Compatible" content="ie=edge">
     <title>Login</title>
 </head>
 <body>
     <form method="POST">
         {% csrf_token %}
         <input type="text" name="auth_id">
         <input type="password" name="auth_pwd">
         <input type="submit" value="Login">
     </form>
 
     <a href="{% provider_login_url 'kakao' method='oauth2' %}">Kakao_login</a>
 </body>
 </html>
 ```




### Kakao

 Kakao Developers

 <https://developers.kakao.com/apps>

 CallBack URL :

 <http://localhost:8000/accounts/kakao/login/callback/>


### Image 부가설명

카카오 개발자 사이트에 들어가서 사용할 `내 애플리케이션`을 먼저 생성

{% asset_img slug kakao_allauth_1.PNG %}

생성을 하고나면 위와 같이 키를 준다. 필자가 사용하고자 하는 키는 `REST API키`이다.

{% asset_img slug kakao_allauth_2.PNG %}

사용자 관리를 들어가서 `사용자 관리 ON`을 해주고 받을 정보들을 설정해준다. 검수작업이 필요할 때도 있기에 이로 인해 사용가능 시간이 걸리는 것 같기도 하다...

{% asset_img slug kakao_allauth_2_2.PNG %}

가장 중요한 곳이다. 일반으로 들어가서 애플리케이션을 사용하고자 하는 곳을 설정. 필자는 웹으로 사용하기에 웹을 설정하고 local에서 진행하였기에 local주소를 설정하였다.
`Redirect Path`는 정해져있다고 보면 된다.

{% asset_img slug kakao_allauth_3.PNG %}

`고급설정`에서 Secret키를 가져온다. 위에 가져왔던 REST_API키와 Secret키로 아래 설정을 해주면 된다.

{% asset_img slug kakao_allauth_4.PNG %}
<br/>
{% asset_img slug kakao_allauth_5.PNG %}

설정은 admin에 들어가서 해주면 되고, 아래는 설정이 끝난 다음 테스팅한 과정이다.

{% asset_img slug kakao_allauth_6.PNG %}
<br/>
{% asset_img slug kakao_allauth_7.PNG %}

kakao_login을 누르면 아래와 같이 kakao 로그인 페이지가 나온다.

{% asset_img slug kakao_allauth_8.PNG %}

접속을 하면 개발자가 redirect시킨 곳으로 들어가게 된다.

{% asset_img slug kakao_allauth_9.PNG %}

이로 인하여 kakao_oauth2_REST KEY방식을 완성하였다.

### SourceCode
<https://github.com/TaeJuneJoung/Python/tree/master/library/oauth_project>