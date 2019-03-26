---
layout: post
title: "JavaScript 기본 정리"
categories:
  - JavaScript
tags:
  - JavaScript
---

## JavaScript 란

웹 페이지를 구성요소에는 HTML, CSS, Javascript 가 있습니다.

HTML 은 구조를 만들어 주고 CSS 는 HTML에 의해 만들어진 요소들에 스타일을 지정합니다.

이렇게 만들어진 웹페이지를 동적으로 컨텐츠를 바꾸기도 하고 

이벤트도 걸어주며 나아가 이제는 서버사이드 언어로 사용되고 있습니다.



## JavaScript 기본 문법

### 변수의 선언과 초기화

메모리에 데이터 공간을 할당하는 것을 **변수의 선언**이라고 합니다.

선언된 공간에 값을 넣는것을 **대입**이라고 합니다.



> 자바스크립스트는 느슨한 타입(loosely typed) 언어이며 동적(dynamic) 언어이기 때문에 변수의 타입을 미리 선언할 필요가 없습니다.



자바스크립스트에서는 다음과 같이 `var` 키워드[^1]를 사용하여 변수를 선언합니다.

[^1]: 키워드는 제어문의 시작과 끝, 특정한 조작 목적 등에 쓰입니다. [키워드와 예약어](https://blog.sonim1.com/118) 참조.

```javascript
var man; // 변수의 선언
var num = 123; // 변수의 선언과 초기화
```

**변수의 선언**은 데이터 공간을 `man` 이라는 변수명으로 사용하겠다는 것을 의미합니다.

`num` 은 이라는 공간에  `123` 이라는 값을 변수의 선언과 동시에 대입시키는 것을 **초기화**라고 합니다.



> 문장 끝에 ;(세미콜론)은 문장이 끝난다는 것을 컴퓨터에게 알려주는 것입니다.
>
> 선택사항이기 때문에 사용하는 건 개인적인 판단입니다.
>
> 자세히 알고싶은 분들은 [세미콜론을 써야 하나 말아야 하나](https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html) 를 읽어 보시는 것을 추천드립니다.



### 자료형

자료형은 다음과 같이 7가지 형태가 있습니다.

- 기본 자료형 (Primitive)
  - **Boolean** - `true` 또는 `false` 중 하나를 값으로 가집니다.
  - **Null** - 어떤 값이 의도적으로 비어있음을 표현한 것입니다.
  - **Undefined** - 값을 할당하지 않은 상태를 표현한 것입니다.
  - **Number** - 숫자를 값으로 가집니다.
  - **String** - 문자를 값으로 가집니다.
  - **Symbol** (ECMAScript 6 에 추가됨)
- **Object**
  - 데이터를 다루는 데이터 구조
  - Array, Function 등 ..



### 주석

주석은 설명을 덧붙일때 사용합니다.

참고로 주석은 브라우저에 표시되기 때문에 중요정보를 포함하지 않는것이 좋습니다.

다음과 같이 **단일행 주석, 다중행 주석, 문서화 주석**을 사용할 수 있습니다.

```javascript
// 이건 단일행 주석입니다

/* 문단을 주석 
   처리할 수 있는
   다중행 주석입니다.
*/

/**
*	문서화 주석입니다.
*/
```



## 참조 사이트

[생활코딩](https://opentutorials.org/course/1)

[zerocho](https://www.zerocho.com/)

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript)

