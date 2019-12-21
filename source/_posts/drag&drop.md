---
title: JS] Drag & Drop
date: 2019-08-07 02:47:00
tags: ['Drag&Drop']
categories: ['JS']
---

학습목표 :

**나의 Todo List를 `Todo, Doing, Finish`로 나누어 해당 상황에 맞게 옮겨 관리하기**

Element를 drag할 수 있게 속성 `draggable="true"`를 설정해줘야 한다.

**ondragstart**

: drag할 데이터를 지정하는 drag(event)함수를 호출

```js
drag: function(event) {
    //()"text":데이터 유형, drag된 데이터의 값)
    event.dataTransfer.setData("text", event.target.id);
}
```

**ondragover**

: drag한 데이터를 놓을 수 있는 위치 지정

기본적으로 데이터/요소는 다른 요소에 놓을 수 없기에, drop을 허용하려면 기본 처리를 방지해야한다.`preventDefault()`

```js
allowDrop: function(event) {
    //drop허용하기 위해 기본처리 방지
	event.preventDefault();
}
```

**ondrop**

```js
drop: function(event) {
	if ((this.doto_type_list).includes(event.target.id))  {
		event.preventDefault();
		let data = event.dataTransfer.getData("text");
		event.target.appendChild(document.getElementById(data));
    } else {
		event.preventDefault();
		let data = event.dataTransfer.getData("text");
		event.target.parentNode.appendChild(document.getElementById(data));
    }
}
```


### 해결해나가야하는 문제

1. `appendChild`라서 다른 곳으로 전체를 이동 후, 마지막에 옮겨 맨 아래에 있는 요소를 해당 옮긴 라인 상위로 올릴 수가 없음.
2. DB와 연동하여 값이 읽고 쓰기가 가능해야함
3. dataTransfer.getData와 setData에 대해 알아보기


### 참고사이트

https://www.w3schools.com/html/html5_draganddrop.asp