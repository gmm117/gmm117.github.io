---
layout: post
title: Develop Concept
date: 2020-07-12 00:00:00 +0200
published: 2020-07-12 00:00:00 +0200
categories: development
tags: ['AMD-WEBPACK Migration']
comments: true,
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    AMD-WEBPACK Migration
</pre>
<!--more-->

---

# AMD-WEBPACK Migration

## AMD vs COMMONJS
AMD와 Common.js는 모듈화된 코드를 로딩하는 역할을 한다. 

이 둘의 차이점은, 
모든 모듈의 로딩이 완료된 후에 실행하는 것인가 
아니면 로딩 완료 이전에 실행하는 것인가이다. 

### COMMONJS

모든 모듈이 로컬에 다운로드가 된 이후에 실행하는 방식이다. 
node.js에서 사용하는 방식으로
server 환경에서 외부 모듈을 가져올 때 유리한 방식이다.

{% highlight javascript %}
// commonjs
var lib = require("package/lib");

function foo() {
  lib.log("hello world!");
}
exports.foobar = foo;

{% endhighlight %}

하지만 모든 모듈을 다운로드할 때까지
페이지를 동작할 수 없다는 것이 단점으로 부각되어
이를 해결하기 위해 새롭게 탄생한 방식이 AMD 방식이다. 

### AMD

비동기적으로 필요한 파일을 다운로드하는 방식으로 
client단(브라우저 환경)에서 외부 모듈을 가져올 때 유리한 방식이다.

{% highlight javascript %}
// amd
define(["package/lib"], function (lib) {
 function foo() {
  lib.log("hello world!");
 }

 return {
   foobar : foo
 }
}
{% endhighlight %}

### UMD

참고로 어떤 방식으로 외부 모듈을 로딩하는 것과 무관하게
각 모듈을 선언하는 방식을 UMD 방식이라고 한다. 

{% highlight javascript %}

(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery', 'underscore'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS-like
        module.exports = factory(require('jquery'), require('underscore'));
    } else {
        // Browser globals (root is window)
        root.returnExports = factory(root.jQuery, root._);
    }

}(this, function ($, _) {
    //    methods
    function a(){};    //    private because it's not returned (see below)
    function b(){};    //    public because it's returned
    function c(){};    //    public because it's returned

    //    exposed public methods
    return {
        b: b,
        c: c
    }
}));

{% endhighlight %}

<h1 style="font-weight:bold">예제코드</h1>
<a href="https://iam-song.tistory.com/28" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://iam-song.tistory.com/28</a>

<h1 style="font-weight:bold">참고사이트</h1>

<a href="https://iam-song.tistory.com/28" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://iam-song.tistory.com/28</a>
<a href="http://d2.naver.com/helloworld/12864" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">http://d2.naver.com/helloworld/12864</a>

## ES5(AMD) to ES2015~ES2020 Migration

### return => export default

{% highlight javascript %}

//////// ES5(AMD) ////////////
define([  
], function () {
    "use strict";
    var utils = function() {

    }

    return {
        GetName: function() {
            return "Hong's App";
        }
    };
});

//////// ES2015~ES2020 ////////////
// define([  
// ], function () {
    // "use strict";    

    var utils = function() {
        "use strict";
    }

    export default {// return {
        GetName: function() {
            return "Hong's App";
        }
    };
// });

{% endhighlight %}

### require => import

{% highlight javascript %}

//////// ES5(AMD) ////////////
define(function (require) {
	"use strict";
    
    var utils = require('./utils');

	/**
	 * @param 
	 * @constructor 
	 */
	function App() {
        
    }
    
    return {
        GetName: function() {
            return utils.GetName();
        }
    };
});


//////// ES2015~ES2020 ////////////
// define(function (require) {
import utils from 'utils'; //var utils = require('./utils');
    // "use strict";

	/**
	 * @param 
	 * @constructor 
	 */
	function App() {
        "use strict";
    }
    
    export default {// return {
        GetName: function() {
            return utils.GetName();
        }
    };
// });

{% endhighlight %}

### define => import

{% highlight javascript %}

//////// ES5(AMD) ////////////
define([
    'calc',
    'app'  
], function (_calc, _app) {
    var calc = new _calc();
    var app = _app;

    console.log(calc.sum(1, 2));
    console.log(app.GetName());
});

//////// ES2015~ES2020 ////////////
import '../index.css'; // css 있는경우에는 import 해준다.

//define([
import _calc from 'calc';
import _app from 'app';

//], function (_calc, _app) {
    var calc = new _calc();
    var app = _app;

    console.log(calc.sum(1, 2));
    console.log(app.GetName());
//});

{% endhighlight %}

### require config => webpack resolve
{% highlight javascript %}

//////// ES5(AMD) ////////////
require.config({
    // 모듈의 기본 경로를 설정 
    'baseUrl' : 'src',
    paths: {
        calc: './modules/calc'
    }
});

require(['index'], function() { 
    
});

{% endhighlight %}

{% highlight webpack %}
    resolve: {
        modules: [
        path.join(__dirname, "src"), // config baseurl
        "node_modules"
        ],
        alias: { // config path 별칭대체
        'calc': './modules/calc'
        }
    }
{% endhighlight %}

### Transformations

#### Exporting Modules
{% highlight javascript %}
//////// ES5(AMD) ////////////
define(function () {
    return function () {};
});
//////// ES2015~ES2020 ////////////
export default function () {}
{% endhighlight %}

#### Importing Modules
{% highlight javascript %}
//////// ES5(AMD) ////////////
define(['module-name'], function (module) {});

//////// ES2015~ES2020 ////////////
import module from 'module-name';

//////// COMMONJS ////////////
define(function(require) {
  var module = require('module-name');
});
{% endhighlight %}

#### Dynamic Imports
{% highlight javascript %}
//////// ES5(AMD) ////////////
require([
  'module-name-1',
  'module-name-2'
], function (module1, module2) {});
//////// ES2015~ES2020 ////////////
Promise.all([
  import('module-name-1'),
  import('module-name-2')
]).then(([module1, module2]) => {});

////////// dynamic expressions in import ///////
import('./folder/module-name-' + dynamic).then((module) => {});
{% endhighlight %}

---

<h1 style="font-weight:bold">참고사이트</h1>

<a href="https://arthuryidi.com/migrate-amd-modules/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://arthuryidi.com/migrate-amd-modules/</a>