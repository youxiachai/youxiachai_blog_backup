title: JS 数组的奇妙用法
date: 2014-06-09
tags: node
---

## 前言
一般语言的数组的大多除了存数值,还是存数值,不过对于js而已,数组的用法异常的灵活,本博文,就简单介绍几种技巧.

<!--more-->

## 函数队列

在敲代码,我们首先要学习JS对象的两个方法,这两个方法在整个js编程中非常重要!

* call [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
* apply [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

阅读完`call`和`apply` 的用法后,就开始我们的实例演练.

有这么一个关于积分规则的需求

1. 登录5分
2. 发帖1分
3. 被删帖-10分

嗯,思考了十秒以后,你应该会敲下如下代码

```js
var totalPoint = 0;
var action = ''
switch (action) {
    case '登录':
        totalPoint +=5;
        break;
    case '发帖':
        totalPoint +=1;
        break;
    case '被删帖':
        totalPoint -=10;
        break;
    default :

}
```

非常直观合理,简单快速实现了我们的需求,增加规则,我们只要增加case就好了.......

不过十几日后..

你的PM想到了20条规则(还有诸如,发帖了10个贴就不能增加积分之类的,触发机制.),于是你的`switch`代码有接近20条以上的分支,而且,每个分支的积分规则都要注意不要放错位置.

还记得,我们前面学习的`call` 和 `apply` 吗,那现在我们用js数组就能够优雅的处理好.


约定:

* 每个规则设定的变量都是一个拥有一个参数,和一个有两个参数的函数.
* 每个规则参数的变量都是一个拥有长度为2的数组,第一个为对象,第二个拥有两个参数的函数.

```js
//规则设定
var taskQueue = [];
//规则参数
var ruleQuery = [];

var totalPoint = 0;

//参数预置和结果输出
var rule1 = ['', function (result) {
	console.log(result);
}];


ruleQuery['rule1'] = rule1;

//积分规则

var task1  = function (params, done) {
	done(totalPoint += 5);
}

taskQueue['登录'] = task1;

var action = '登录'

//运行预定规则任务
taskQueue[action].apply(null, ruleQuery['rule1']);
> 5
```

乍看,好像比`switch`的方式复杂不小,这种写法的好处,就是我们只需要定义规则和规则参数,并且填入到数组里面就行,不像`switch`要关心一大坨case 分支.

例如刚才提到的
> 帖了10个贴就不能增加积分之类的

我们只需要稍稍改良一下`task`的定义
```js
var totalPoint = 0;

var ruleQuery = [];

var taskQueue = [];

//rule第一个参数可以用于定义发帖的计量.
var rule1 = [1, function (result) {
    console.log(result);
}];


ruleQuery['rule1'] = rule1;

//积分规则
var task2  = function (params, done) {
    if(params < 10){
        totalPoint += 5;
    } else {
        totalPoint += 0;
    }

    done(totalPoint);

}

taskQueue['发帖'] = task2;

taskQueue['发帖'].apply(null, ruleQuery['rule1']);
```



