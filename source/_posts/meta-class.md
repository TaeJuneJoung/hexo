---
title: Meta Class
date: 2019-08-08 23:39:15
tags: ['Meta Class']
categories: ['Python']
---

# Meta Class

클래스 -> 인스턴스 객체 생성

메타클래스 -> 클래스 객체 생성



**type(name, bases, dict)**

> `type()` 함수의 목적은 타입을 알아보는 것도 있지만,
>
> **새로운 클래스를 만드는 메타클래스의 목적**도 있다.
>
> name : 만들 클래스 이름
>
> bases : 부모 클래스의 튜플
>
> dict : 속성 값을 정의하는 심볼 사전

```python
class Klass:
    a = 10
    b = 20
Klass = Klass()
print(Klass) #<__main__.Klass object at 0x0...>

Klass2 = type('Klass2', (), {'a':10, 'b':20})
print(Klass2) #<class '__main__.Klass2'>

print(Klass.a, Klass2.a) #10 10
print(type(Klass), type(Klass2)) #<class '__main__.Klass'> <class 'type'>
```



**-상속 구현**

```python
class Klass:
    a = 10
    b = 20

#Inheritance가 Klass를 상속받음
class Inheritance(Klass):
    pass

inheritance = type('Inher', (), {})
print(inheritance) #<class '__main__.Inher'>
print(inheritance.__bases__) #(<class 'object'>,) -베이스 클래스 확인
```



**-클래스 동적 구현**

```python
class Klass():
    a = 10
    b = 20

def display(self):
    print(self.a)

Dynamic = type('Dynamic', (Klass,), {'display':display})
res = Dynamic()
print(res.display) #<bound method display of <__main__.Dynamic object at ...>>
print(Dynamic.mro()) #[<class '__main__.Dynamic'>, <class '__main__.Klass'>, <class 'object'>]
```



**-메타클래스 생성**

```python
class SubType(type):
    def __init__(class_object, name, bases, class_dict):
        print('__init__', name)
        super().__init__(name, bases, class_dict)

Sub1 = SubType('SubMetaClass', (), {}) #__init__ SubMetaClass
print(Sub1) #<class '__main__.SubMetaClass'>

#Method가 있는 클래스 생성
Sub2 = SubType('SubMetaClass2', (), {'foo':lambda self: 'bar'}) #__init__ SubMetaClass2
sub = Sub2()
print(Sub2) #<class '__main__.SubMetaClass2'>
print(sub) #<__main__.SubMetaClass2 object at 0x...>
print(sub.foo()) #bar
```



**-메서드 자동 추가**

```python
#__new__()
class SubType(type):
    def __new__(class_object, name, bases, class_dict):
        return type.__new__(class_object, name, bases, class_dict)

class SubType2(type):
    def __new__(class_object, name, bases, class_dict):
        class_dict['foo'] = lambda self: 'bar' #add method
        return type.__new__(class_object, name, bases, class_dict)

Sub = SubType2('SubMetaClass', (), {})
res = Sub()
print(res.foo()) #bar
```

`type`을 `super`로 바꾸면 안됨.



`__new__()` 메서드로 전달되는 클래스와 `__init__()`메서드로 전달되는 첫 인수는 다른 객체이다.

```python
class SubType(type):
    def __new__(class_object, name, bases, class_dict):
        print('[__new__]', class_object.__name__, name) #[__new__] SubType SubMetaClass
        return type.__new__(class_object, name, bases, class_dict)
    def __init__(class_object, name, bases, class_dict):
        print('[__init__]', class_object.__name__, name) #[__init__] SubMetaClass SubMetaClass
        type.__init__(class_object, name, bases, class_dict)

Sub = SubType('SubMetaClass', (), {})
```

`__new__()` : 메타클래스

`__init__()` : 클래스 객체



메타클래스에 정의된 모든 클래스는 첫 인수로 생성된 클래스 객체를 받는다.

-> 메타클래스의 메서드는 생성된 객체(클래스)를 인수로 받기 때문

```python
class Klass(type): #Meta Class
    def who_am_i(class_name):
        #class_name에 self를 써야하나 메타클래스의 메서드는 생성된 객체를 첫 인자로 받기에
        print(f"I am {class_name}")

foo = Klass('Foo', (), {})
print(foo.who_am_i) #<bound method Klass.who_am_i of <class '__main__.Foo'>>
foo.who_am_i() #I am <class '__main__.Foo'>
Klass.who_am_i(foo) #I am <class '__main__.Foo'>
```

