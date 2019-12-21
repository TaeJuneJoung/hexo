---
title: Python] SMTP 이메일 보내기
date: 2019-11-10 10:26:32
tags: ['Python', 'SMTP']
categories: ['Python']
---

## SMTP

**Simple Mail Transfer Protocol**

: 인터넷 상의 유효한 이메일 아이디로 이메일을 보내는데 사용되는 클라이언트




## IMAP 설정

{% asset_img slug smtp01.png %}

설정안함을 설정함으로 바꿔주면 된다.
이 설정만 하면은 Gmail의 보안규칙을 다 지키진 못한다.


```bash
socket.gaierror: [Errno 11001] getaddrinfo failed
```

무슨 문제인가 했더니, 다른 부분으로 설정하기도 하였고 `app password`를 사용하지도 않아 Gmail 정책 보안상 문제가 일어났던 부분이 있었던거 같다.


## APP Password 설정

{% asset_img slug smtp02.png %}

계정설정으로 들어간 후, 보안에서 2차 비밀번호를 설정하고 앱 패스워드를 설정하면 된다.


{% asset_img slug smtp03.png %}

앱 패스워드는 16자로 주워지며, Email에 대한 부분을 사용할 것이기에 필자는 `메일 / Windows 컴퓨터`로 설정하였다.
설정이 끝나서 받은 앱 패스워드를 이제 사용할 것이기에 잊어버리면 안된다.


## 메일 보내기

기본적인 설정이 끝났으니, 이제 메일을 보내도록 하겠다.

```python
import smtplib
from email.mime.text import MIMEText
# STMP 세션 생성
smtp = smtplib.SMTP('smtp.gmail.com', 587) #Gmail Port: 587
# TLS 보안 start
smtp.starttls()

smtp.login('gmail email', 'app password')

msg = MIMEText('내용 : content')
msg['Subject'] = '제목 : Title'

smtp.sendmail('발신자 이메일', '수신자 이메일', msg.as_string())

# 세션 종료
smtp.quit()
```



### 참고자료

 https://yeolco.tistory.com/93 