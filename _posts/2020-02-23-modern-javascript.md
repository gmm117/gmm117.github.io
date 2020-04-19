---
layout: post
title: modern javascript
date: 2020-02-23 00:00:00 +0200
published: 2020-02-23 00:00:00 +0200
categories: development
tags: ['javascript', 'es6~9']
comments: true,
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    Default Values
    template literals
    HTML Fragments
    Syled Components
    More String Implovements
    Array
    Destructuring
    Rest and Spread
    For ... of
    classes
    promise/async,await/fetch
    Symbol/Map/Generator
</pre>
<!--more-->

---

# Default Values
```javascript
function sayHello(aName = "hong") {
  return "Hello " + aName;
}

console.log(sayHi());

* 기존 JS6이전에는 aName이 undefined 여부를 체크한 이후에 다시 값을 세팅해야했는데, 인자에 대한 초기값 세팅이 가능해졌다.
예) let defalutName = aName || "hong"

# template literals
```javascript
// JS6이전
let sayHello1 = (aName="hong") => "hello " + aName;
console.log(sayHello1());

// JS6이후
let sayHello2 = (aName="hong") => `hello ${aName}`;
console.log(sayHello2());

console.log(`hello ${100*100}`);

const add = (prev, next) => prev + next;
console.log(`prev plus next : ${add(3,4)}`);
```

## HTML Fragments
```javascript
// JS6이전
let text1 = "HELLO HONG";
let body = document.body;
let div = document.createElement('div');
let h1 = document.createElement('h1');
h1.innerText = text1;
div.appendChild(h1);
body.innerHTML = div.innerHTML;

// JS6이후
let text1 = "HELLO HONG";
let body = document.body;
let div = `
  <div>
    <h1>${text1}</h1>
  </div>
`
body.innerHTML = div;
```

## Syled Components
```javascript
const styled = (newElement) => {
	const el = document.createElement(newElement);
	return (args) => {
      el.style= args[0];
      console.log(args[0]);
      return el;
    }
};

var element = styled('h1')`
	border-radius : 10px;
	color:blue;
`;
console.log(element);
```

# More String Implovements
```javascript
const isEmail = email => email.includes("@");
console.log(isEmail("gmm117@naver.com"));

const lastNumber = '2519';
console.log(`${'*'.repeat(3)}-${'*'.repeat(4)}-${lastNumber}`);

const name = "hong";
console.log(name.startsWith("o"));
console.log(name.endsWith("g"));
```

# Array

## Array.of, Array.from
```javascript
Array.of(1, 2, 3, false, "hong");
// Array.from => array-like object(HTMLCollection등등) 를 array 만들어줌
```
## Array.find
```javascript
const friendEmails = [
	"a@gmail.com",
	"b@naver.com",
	"c@daum.net"
];

const target = friendEmails.find(friend => friend.includes("gmail.com"));
console.log(target);

const targetIdx = friendEmails.findIndex(friend => friend.includes("gmail.com"));
console.log(targetIdx);

friendEmails.fill("*".repeat(5), 0, 2);
console.log(friendEmails);
// value : 배열을 채울 값, start 시작인덱스, end 끝인덱스(숫자의 -1인덱스))
```

# Destructuring

## Object Destructuring
```javascript
// 비구조화 할당
const settings = {
  noti : {
    follow : true,
    alerts : true,
    unfollow : false
  },
  color : {
    theme : "dark"
  }
};

const {
  noti : {
    data = "init",
    unfollow
  } = {},
  color : {
    theme
  }
} = settings;
console.log(unfollow, theme, data);
```

## Array Destructuring
```javascript
const days = ["Mon", "Tue", "Wen", "Thu", "Fri", "Sat"];

// 이전
// const Mon = days[0];
// const Tue = days[1];
// const Wen = days[2];
// const Thu = days[3];
// const Fri = days[4];
// const Sat = days[5];
// const Sun = days[6];

// 이후
const [Mon, Tue, Wen, Thu, Fri, Sat, Sun = "Sun"] = days;
console.log(Mon, Tue, Wen, Thu, Fri, Sat, Sun);
```

# Rest and Spread

## Spread
```javascript
// Spread object/Array unpack
const number = [1, 2, 3, 4];
const alpha = ['a', 'b', 'c'];

console.log([...number, ...alpha]);
//[1, 2, 3, 4, "a", "b", "c"]

const person1 = {
  name : 'hong',
  age : 33
};

const person2 = {
  name1 : 'park',
  age1: 33
};

console.log({...person1, ...person2});
// {name: "hong", age: 33, name1: "park", age1: 33}

const friends = ['choi', 'kim'];
const newfriends = [...friends, 'seyoung'];

console.log(newfriends);
// ["choi", "kim", "seyoung"]

const choi = {
  username : 'choi'
};

console.log({...choi, password:'***123***'});
//{username: "choi", password: "***123***"}
```

## Rest
```javascript
const bestfriends = (one, ...friendsRest) => console.log(`my best friends : ${one}, friends rest : ${friendsRest}`);

bestfriends('kim', 'choi', 'seyoung');
```

## Rest & Spread Destructure 
```javascript
// object 삭제 & 정리할경우 유용
const user = {
  name : 'hong',
  age : 36,
  password : '**123'
};

const ignorepwd = ({password, ...rest}) => rest;

console.log(ignorepwd(user));

// object를 만들어서 return 할 경우 () 감싸줘야한다.
const setCountry = ({country="kr", ...rest}) => ({country, ...rest}); // ({country, ...rest}) spread 문법사용

console.log(setCountry(user));

const user1 = {
  NAME : 'hong',
  age : 36,
  password : '**123'
};

// const 변수를 새로만들듯이 새로운 변수를 세팅
const rename = ({NAME:name, ...rest}) => ({name, ...rest});

console.log(rename(user1));
```

# For ... of
```javascript
const friends = ['kim', 'choi', 'seyoung', 'duhyun'];

// 잘못된 length의 array에 접근시 undefined가 나오는 문제는 생긴다.(ex: i<20)
for(let i=0; i<friends.length; ++i) {
  console.log(`my best friends ${i}, ${friends[i]}`);
}

const myFriends = (friend, i) => console.log(`my best friends ${i}, ${friend}`)

friends.forEach(myFriends);

// const, let 선언 가능(forEach에 비해서)
// 
for(const friend of friends) {
  console.log(`my best friends ${friend}`);
}

for(const str of "Helloo this is string") {
  console.log(`${str}`);
}
```

# classes
## Instroduction classes
```javascript
const MakeUser = {
  userName : 'hong',
  sayHello : function() {
    console.log(`hello, this is ${this.userName}`);
  }
}

// class와 그냥 객체의 차이점은 new로 할당할 경우에만 instance를 생성한다는 것이다.(MakeUser를 객체를 만들어서 리턴한 것이다.)
class User {
  constructor(name) {
    this.userName = name;
  }

  sayHello() {
    console.log(`hello, this is ${this.userName}`);
  }
};

const user1 = new User('hong');
const user2 = new User('seungah');

const user3 = MakeUser;

console.log(user1.sayHello());
console.log(user2.sayHello());
console.log(user3.sayHello());
```
## Extending classes
```javascript
class User {
  constructor({name}) {
    this.userName = name;
  }

  sayHello() {
    console.log(`hello, this is ${this.userName}`);
  }
};

// 자식 생성자가 있는경우 super 키워드가 없으면 상속받은 부모 생성자 호출이 불가능하다.
class Admin extends User {
  constructor({name, superAdmin}) {
    super({name});
    this.superAdmin = superAdmin;
  }
  sayAdmin() {
    console.log(`admin, this is ${this.userName} superAdmin : ${this.superAdmin}`);
  }
}

const admin = new Admin({name : 'hong', superAdmin: true});
admin.sayHello();
admin.sayAdmin();
```
##  WTF is this
```javascript
class Counter {
this.plusButton.addEventListener("click", this.increase);
this.minusButton.addEventListener("click", this.descrease);

// ctrl eventHandler callback 호출의 this는 ctrl자체를 가리킨다.(여기선 button)
// 수정전
  increase() {
    consoloe.log(this);
  }

  descrease() {
    consoloe.log(this);
  }

  // 수정후(lexical scope this를 가지고 있습니다.??)
  increase = () => {
    consoloe.log(this);
  }

  descrease = () => {
    consoloe.log(this);
  }
}
```

# Promise

## create promises
```javascript
// async function
const newPromise = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, 'I am new Promise');
});

console.log(newPromise);

setInterval(console.log, 1000, newPromise);
```

## using promises
```javascript
const newPromise = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, 'I am new Promise');
});

// resolve : then, reject : catch 호출이며 단 하나만 호출이 된다.
newPromise
  .then(value => console.log(value))
  .catch(err => console.log(`error ${err}`));
```

## chaining promises
```javascript
  const newPromise = new Promise((resolve, reject) => {
  resolve(2);
});

const timesTwo = number => number * 2;

newPromise
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(lastnumber => console.log(lastnumber));
```

## promises all/race
```javascript

const f1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, "first");
});

const f2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 5000, "secode");
});

const f3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 7000, "third");
});

// Promise.all 시간과 상관없이 순서에 맞게 값을 제공해준다.
const fall = Promise.all([f1, f2, f3]);
fall.then(values => console.log(values));

// Promise.race f1,f2,f3중 가장먼저 resolve, reject 되는 내용의 결과값을 제공해준다.
const frace = Promise.race([f1, f2, f3]);
fall.then(values => console.log(values));
```

## promises finally
```javascript
// resolve, reject이 호출되더라도 finally를 무조건 한번은 타도록 되어있음
const p1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 3000, "before finally");
})
.then(value => console.log(value))
.catch(err => console.log(err))
.finally(() => console.log("call finally"));
```
  
## fetch
```javascript
// fetch의 return값을 promise를 리턴하도록 되어있음
fetch("https://yts.am/api/v2/list_movies.json")
.then(response => response.json())
.then(result => console.log(result))
.catch(err => console.log(err))
```

## Async Await
```javascript
// promise를 사용하게 되면 사용자가 얻고자하는 값이 여러개 일경우 then/then/then을 호출하게 되어서 코드가 복잡해진다.
const getMoviesPromise = () => {
  fetch("https://yts.am/api/v2/list_movies.json")
  .then(response => response.json())
  .then(result => console.log(result))
  .catch(err => console.log(err));
};


const getMoviesAsync = async( ) => {
  const response = await fetch("https://yts.lt/api/v2/list_movies.json");
  const json = await response.json();
  console.log(json);
};

getMoviesAsync();
```

## Async Awaite(try catch finally)
```javascript
const getMoviesAsync = async () => {
  try {
     const response = await fetch("https://yts.lt/api/v2/list_movies.json");
    const json = await response.json();
    console.log(json); 
  } catch(e) {
    console.log(`Error ${e}`);
  } finally {
    console.log("we are done");
  }
};

getMoviesAsync();
```

## Paraller Async Await
```javascript
const getMoviesAsync = async () => {
  try {
     const [moviesRespose, suggestionResponse] = await Promise.all([
       fetch("https://yts.lt/api/v2/list_movies.json"),
       fetch("https://yts.lt/api/v2/movie_suggestions.json")
       ]);
    const [movies, suggestion] = await Promise.all([
       moviesRespose.json(),
       suggestionResponse.json()
    ]);
    console.log(movies); 
    console.log(suggestion); 
  } catch(e) {
    console.log(`Error ${e}`);
  } finally {
    console.log("we are done");
  }
};

getMoviesAsync();

```


## Symbol
```javascript
// uniquie함을 보장해준다.
const info = {
  [Symbol('hong')] : {
    age : 35
  },
  [Symbol('hong')] : {
    age : 30
  },
  hello : "bye"
};

// key에 대한 privacy 보장
Object.keys(info);
Object.getOwnPropertySymbols(info); // private을 보장하지 않는다.
```

## Sets
```javascript
// Sets
// 중복된 값을 저장하지 않는다.
const userset = new Set([1, 2, 3, 4, 5, 6, 7, 7,7,7,8]);
userset.has(9);
userset.delete(4);
userset.clear();
userset.add('hi');
userset.size
userset.keys(); // return iterator
```

## WeakSet
```javascript
// weakset은 number, text 저장 불가능(단지 objects와 함께 동작)
// weakset은 has,add,delete 정도만 가지고 있는 작은 set이다.
// weakset에 넣은 objects를 가리키는 것이 없다면 garbage collection에 의해서 지워진다(약하게 붙들려 있다고 가정)
const weakSet = new WeakSet();
weakSet.add({hi:true});
```

## Map
```javascript
// map도 weakmap 존재
const map = new Map();
map.set("age", 35);
map.entries();
map.has("age");
map.get("age");
map.set("age", 1111); // 덮어쓰기 가능
```

## Generator
```javascript
// next를 수행하면서 각각 여러 동작들을 순서에 맞게 처리가능
function* listPeople() {
  // 1. 동작
	yield 'hong';
  // 2. 동작
	yield 'kim';
  // 3. 동작
	yield 'choi';
  // 4. 동작
	yield 'park';
};

function* friendTeller() {
	for(const friend of friends) {
    	yield friend;
	}
}

const friends = ['hong', 'kim', 'choi', 'park'];
const friendLooper = friendTeller();

const listG = listPeople();
listG.next();
//{value: "hong", done: false}
listG.next();
//{value: "kim", done: false}
listG.next();
//{value: "choi", done: false}
listG.next();
//{value: "park", done: false}
listG.next();
//{value: undefined, done: true}
```

## Proxies
```javascript
// 속성조회,할당등에 대한 행위에 대한 사용자의 커스텀 동작을 정의할 떄 사용
const userObj = {
  username : 'hong',
  age : 36,
  password : 1234
};

const userObj = {
  username : 'hong',
  age : 36,
  password : 1234
};

const userFilter = {
  get : (target, prop, recevier) => {
    return prop === "password" ? `${"*".repeat(5)}` : target[prop];
  },
  set : () => {
    console.log("call set function");
  }
};

const filteredUser = new Proxy(userObj, userFilter);
undefined
filteredUser.password
// "*****"
filteredUser.age
// 36
filteredUser.username
// "hong"
```

# 그외 
## reduce
**배열.reduce((누적값, 현잿값, 인덱스, 요소) => { return 결과 }, 초깃값);**
```javascript
const oneTwoThree = [1, 2, 3];
// 초기값 세팅 O
result = oneTwoThree.reduce((acc, cur, i) => {
  console.log(acc, cur, i);
  return acc + cur;
}, 0);
// 0 1 0
// 1 2 1
// 3 3 2
result; // 6

// 초기값 세팅 X
result = oneTwoThree.reduce((acc, cur, i) => {
  console.log(acc, cur, i);
  return acc + cur;
});
// 1 2 1
// 3 3 2
result; // 6 
```


---
> # Javascript 정리

# IIFE
```javascript
// 숨기고 싶은 값이지만 브라우저에서 secretUsers에 접근이 가능하다.
const secretUsers = ['hong', 'kim', 'choi', 'seyoung'];
console.log(secretUsers);

// IIFE(Immediately-Invoked Function Expressions) 적용
(/*function()*/() => {
  const secretUsers = ['hong', 'kim', 'choi', 'seyoung'];
  console.log(secretUsers);
})()
```
# javascript를 통한 모듈번들러(웹팩,gulp) 기능 간단구현(ES6)
```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
    <!-- type:module로 인해서 import,export 구현가능 -->
    <script type="module" src="app.js"></script>
    <script type="module" src="app2.js"></script>
</body>
</html>
```

```javascript
// app.js
let users = ['hong', 'kim', 'choi'];
export const addUsers = (user) => users = [...users, user];

export const getUsers = () => users;

export const deleteUsers = (user) => users = users.filter(aUser => aUser !== user);
```

```javascript
//app2.js
import {addUsers, getUsers} from "./app.js"

console.log(getUsers());
addUsers("seyoung");
console.log(getUsers());
```
# time

## setTimeout
```javascript
// setTimeout은 시간이 지난후에 기능을 메시지(message queue)에 넣어줌(스택에 빌경우 실행)

setTimeout(() => console.log('setTimeout start'), 1000);
const helloT = setTimeout(console.log, 1000, 'setTimeout start');
clearTimeout(helloT);
```

## setInterval
```javascript
// Interval이 1초보다 적다면 크롬에서는 1초로 강제로 변경
const helloT = setInterval(console.log, 5000, 'setTimeout start');

clearInterval(helloT);
```

## requestAnimationFrame
```javascript
// 이전에는 사물의 움직이거나할 경우 setInterval을 사용했지만, cpu/그래픽카드 같은 물리적인 장치가 느릴 경우 Internal이 느려지는 문제가 있었음
// 브라우저가 자체 프레임을 업데이트(repaint) 하기전에 호출이되서 처리되도록 하는 기능(cpu, 그래픽카드 최적화, 현재탭에서만 처리)
requestAnimationFrame(() => console.log('animation frame'));
```

# Message Queue and Event Loop
```javascript
// 자바스크립트는 디폴트로 갖고 있는 않고 WebApi를 통해서 함수호출이 가능하다.
// stack -> webapi -> queue -> stack 비워질경우 queue에 데이터를 가져와서 stack에서 로딩 후 실행된다.
setTimeout(() => console.log("hi"), 0);
console.log("bye");
```
![setTimeout](/assets/images/{{page.id}}/setTimeout.png)


# Javascript6~10 참고사이트
* [Nomad Courses](https://academy.nomadcoders.co/)