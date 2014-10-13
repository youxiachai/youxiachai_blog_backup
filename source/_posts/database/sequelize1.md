title: Node.js ORM 框架 sequelize 入门教程 1
date: 2014-04-06
tags: [nodejs, sequelize]
---

## 导言

现在Node.js 上面的关系数据库ORM 框架有

* [sequelize](https://github.com/sequelize/sequelize)
* [node-orm2](http://dresende.github.io/node-orm2/)
* [knex](http://knexjs.org/)

其中以sequelize的关注度最高...因为,当初,我就是看着sequelize 的star 数最高..于是就入坑了..对了,国内的话,前端乱炖的数据库ORM用的就是sequelize

<!--more-->

## Sequelize 历史

对于我来说从2013年初到现在 接触sequelize应该有接近一年了..在这过去一年里sequelize的进步还是挺大的....在以前的版本.sequelize 对于关联表关系有着一些异常奇异的问题,这也导致,后边sequelize 的几个作者,下定决心做好单测...有兴趣的朋友可以去围观一下sequelize的单元测试,就现在而言,sequelize应该可以算能用了,所以,也决定写些东西推广一下sequelize,让一些人更加方便的入坑...

在sequelize开发历史中,在1.7 的时候,开了一个2.0 的分支,这两个分支的关系,可能有些人就混乱了,这里说一下,1.7 和 2.0 的版本是同步开发的,在1.7 正式出来的时候,1.7 和 2.0 的代码是一样的.不过,现在1.7已经正式发布,1.7和2.0 可能开始不同步了,所以,大家为了省事还是用1.7吧,至于2.0什么时候开发完..我估计得今年年底吧.

## Let's start!

先安装一下sequelize 的命令行工具,方便一些初始化工作

> Windows ```npm install -g sequelize```
> *nix ```sudo npm install -g sequelize```

安装完以后,我们之间在项目目录运行

> ```sequelize -i```

配置文件就会自动帮我们生成好了

```bash
├───config
└───migrations
```

config 目录下的config.json 就是我们需要根据数据的配置进行修改的文件了

接下来我们安装用到的包

```bash
npm install sequelize --save
npm install sqlite3 --save
npm install mysql --save
```

sequelize 是支持多方言数据库(MySQL, MariaDB, SQLite and PostgreSQL)的ORM 框架.其中对于MariaDB 的支持..貌似就这一家了(不过用MariaDB应该不多吧).

为了展示sequelize 对于多数据库的支持,我使用两个不同的数据库生产环境用mysql,测试开发用sqlite3.

然后去修改 ```config/config.json```

```js
  "development": {
      "username": null,
      "password": null,
      "database": "database_development",
      "option" : {
          "dialect": "sqlite",
          "storage":  "test.sqlite",
          "define": {
              "timestamps": false,
              "freezeTableName": true
          }
      }
  }
```

对于sqlite3而言,直接把dialect 改为sqlite即可,如果是MySQL的,注意username,password,database一个都不能少!

对于define 里面的东西.这里先演示两个. timestamps 对于表是否自动添加createAt 和 updateAt这两个字段,如果你用不上的话,就关掉吧. freezeTableName, 是冻结表名,默认sequelize会帮你的定义的表名字自动加上s..所以,如果你用不上的话,就关掉吧.关于更多的配置请看[http://sequelizejs.com/docs/latest/usage#options](http://sequelizejs.com/docs/latest/usage#options)

## Let's run!

做了这么一番准备,现在开始让我们开始用sequelize进行数据库访问吧!

```js
var Sequelize = require('sequelize');

var node_env = process.env.NODE_ENV ? process.env.NODE_ENV : 'development';

//根据系统环境载入我们的配置
var config = require('../config/config')[node_env];


var dbStroage = new Sequelize(config.database, config.username, config.password, config.option);



//定义我们的User 表

var User = dbStroage.define('user', {
    username: Sequelize.STRING,
    password: Sequelize.STRING
})

//如果是第一次运行的话,需要用sync 方法创建表
dbStroage.sync()
    .success(function () {
        //用sequelize创建我们第一个用户
        User.create({
            username : 'youxiachai',
            password : '123456'
        }).done(function (err, result){
                console.log(err)
                console.log(result)
            })
    })
    .error(function (err){
            console.log(err);

})
```

对于sequelize的配置运行,就到此结束了,下次我们讨论一下如何用sequelize 做一个Restful风格的应用.

本节完整代码 [https://github.com/youxiachai/sequelize-lesson](https://github.com/youxiachai/sequelize-lesson)








