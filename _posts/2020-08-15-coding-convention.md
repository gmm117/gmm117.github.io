---
layout: post
title: coding convention
date: 2020-08-15 00:00:00 +0200
published: 2020-08-15 00:00:00 +0200
categories: development
tags: ['javascript-convention' ]
comments: true,
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    javascript-convention
</pre>
<!--more-->

---

# javascript-convention

## 파일의 이름은 소문자로 표기한다.
## 변수의 이름은 lowerCamelCase로 표기한다.

## // 이벤트 핸들러 - 이벤트 핸들러는 'on'으로 시작
{% highlight javascript %}
const onClick = () => {};
const onKeyDown = () => {};
{% endhighlight %}

## 함수는 lowerCamelCase로 표기한다.
{% highlight javascript %}
// bad
function MyFunction() {...}

// good
function myFunction() {...}
{% endhighlight %}

## 함수의 이름은 동사 또는 동사구문으로 표기한다.
{% highlight javascript %}
// bad
function whereIsCamera() { ... }

// good
function findCamera() { ... }
function getFoo() { ... } // getter
function setBar() { ... } // setter
function hasCoo() { ... } // booleans
{% endhighlight %}

## 클래스나 생성자의 이름은 PascalCase로 표기한다
{% highlight javascript %}
// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
{% endhighlight %}



## space와 tab을 섞어서 사용하지 않는다.
탭을 이용한 들여쓰기는 하지 않지 않고 공백 문자 2개를 사용한다.

## 배열 복사 시 순환문을 사용하지 않는다.
{% highlight javascript %}
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
{% endhighlight %}

ES5의 환경에서는 Array.prototype.slice를 사용한다. (ES5)
{% highlight javascript %}
// Good
itemsCopy = items.slice();
{% endhighlight %}

## 객체의 프로퍼티가 1개인 경우에만 한 줄 정의를 허용하며, 2개 이상일 경우에는 개행을 강제한다.
{% highlight javascript %}
// Bad - 개행
const obj = {foo: 'a', bar: 'b'}

// Good
const obj = {foo: 'a'};

// Good
const obj = {
  foo: 'a'
};
{% endhighlight %}

## 객체 리터럴 정의 시 콜론 앞은 공백을 허용하지 않으며 콜론 뒤는 항상 공백을 강제한다.
{% highlight javascript %}
// Bad - 개행
const obj = {foo: 'a', bar: 'b'}

// Good
const obj = {foo: 'a'};

// Good
const obj = {
  foo: 'a'
};
{% endhighlight %}

## 객체의 메서드 표현 시 축약 메소드 표기를 사용한다.
{% highlight javascript %}
// Bad
const atom = {
  value: 1,

  addValue: function(value) {
    return atom.value + value;
  }
};

// Good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  }
};
{% endhighlight %}

## 메서드 문법 사용 시 메서드 사이에 개행을 추가한다.
{% highlight javascript %}
// Bad
class MyClass {
  foo() {
    //...
  }
  bar() {
    //...
  }
}

// Good
class MyClass {
  foo() {
    //...
  }

  bar() {
    //...
  }
}
{% endhighlight %}

## 공백

### 키워드, 연산자와 다른 코드 사이에 공백이 있어야 한다.
{% highlight javascript %}
// Bad
var value;
if(typeof str==='string') {
  value=(a+b);
}

// Good
var value;
if (typeof str === 'string') {
  value = (a + b);
}
{% endhighlight %}

### 시작 괄호 바로 다음과 끝 괄호 바로 이전에 공백이 있으면 안 된다.
{% highlight javascript %}
// Bad - 괄호 안에 공백
if ( typeof str === 'string' )

// Bad - 괄호 안 공백
var arr = [ 1, 2, 3, 4 ];

// Good
if (typeof str === 'string') {
  ...
}

// Good
var arr = [1, 2, 3, 4];
{% endhighlight %}

### 콤마 다음에 값이 올 경우 공백이 있어야 한다.
{% highlight javascript %}
// Bad - 콤마 뒤 공백
var arr = [1,2,3,4];

// Good
var arr = [1, 2, 3, 4];
{% endhighlight %}

### 중괄호 사이에는 칸 공백을 사용한다.
{% highlight javascript %}
// bad
const foo = {clark: 'kent'};

// good
const foo = { clark: 'kent' };
{% endhighlight %}

### 블록 스코프에서는 함수 선언식을 사용하지 않는다. (ES5)
{% highlight javascript %}
// Bad
if (condition) {
  function someFunction() {
  
  }
} else {
  function someFunction() {
  
  }
}

// Good
var someFunction;

if (condition) {
  someFunction = function() {
    ...
  }
} else {
  someFunction = function() {
    ...
  }
}
{% endhighlight %}

## 선언과 할당의 분리를 허용하는 경우 선언만 하는 변수는 var을 한 번만 사용하는 방식을 허용한다. (ES5)
{% highlight javascript %}
// Bad - 불필요하게 개행
var foo,
  bar,
  quux;

// Good - 선언만 하는 변수, 한 줄로 연결
var foo, bar, quux;
{% endhighlight %}

## const 선언문을 먼저 그룹화한 다음에 let 선언문을 그룹화한다.
{% highlight javascript %}
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
{% endhighlight %}



<h1 style="font-weight:bold">참고사이트</h1>

<a href="https://velog.io/@cada/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%94%EB%94%A9-%EB%B0%8F-%EB%84%A4%EC%9D%B4%EB%B0%8D-%EC%BB%A8%EB%B2%A4%EC%85%98-1%ED%8E%B8" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://velog.io/@cada/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BD%94%EB%94%A9-%EB%B0%8F-%EB%84%A4%EC%9D%B4%EB%B0%8D-%EC%BB%A8%EB%B2%A4%EC%85%98-1%ED%8E%B8</a>
<a href="https://ui.toast.com/fe-guide/ko_CODING-CONVENSION/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://ui.toast.com/fe-guide/ko_CODING-CONVENSION/</a>


---