---
title: Windows] Hyper-V
date: 2019-08-25 00:27:28
tags: ['Hyper-V', 'Windows']
categories: ['Windows']
---

### Window10에서 Virtual Box 구동시 Error

<br/>
> 제어판 -> 프로그램 -> 프로그램 및 기능<br/>
> -> Windows 기능 켜기/끄기<br/>

<br/>
Hyper-V 설정이 켜 있는 경우 가상화 기능을 독점하기에 가상화를 사용하는 다른 프로그램과 충돌이 일어날 수 있다.<br/>
<br/>
Docker를 Window버전으로 다운받아 쓰다가 VirtualBox를 써서 Linux를 사용하려고 하였는데 Hyper-V문제로 작동되지 않았다.<br/>
Hyper-V를 위의 경로로 찾아가 끄고 진행하였더니 블루스크린이 나오고 결국은 복원을 눌러 다시 On이 되었다.<br/>
<br/>
<br/>
Hyper-V가 윈도우 부팅시 자동으로 실행되지 않게 하기 위해

```bash
bcdedit /set hypervisorlaunchtype off
```
<br/>

원래 설정으로 복원하기 위해서는
```bash
bcdedit /set hypervisorlaunchtype auto 
```
<br/>
명령어를 입력하면 된다.