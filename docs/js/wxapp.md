# 小程序

小程序（微信）是一种全新的连接用户与服务的方式，它可以在某个应用（微信）内被便捷地获取和传播，同时具有出色的使用体验。

## 特点


* 原生的体验

* 特色的能力

* 小程序DSL

###实现思路

如何实现原生的体验？ 很简单，开发原生的应用自然具有原生的体验。

不会ios和安卓开发？ 很简单，react-native知道不？

如何实现小程序的DSL开发出原生体验的应用？ 很简单，如何你听说过代码编译的话。

代码编译？ 很简单，把一段代码转化成另一段代码。例如： var a = 1;  => int a = 1;

特色的能力？这个更简单了，如何你听说过JSBridge。

JSBridge？小伙子简单来说就是web和native之间的桥梁,有了它你就可以在web页面中唤起native应用提供的能力。例如：唤起相册，获取应用登陆用户信息等等。

如何开发JSBridge？ 😂  我目前没有了解过，小程序的框架实现仅需要调用。

**综上所述：小程序框架 = 编译器 + JSBridge + 一些特色组件的实现**

一些文档：

[react-native](https://reactnative.cn/)

[微信小程序](https://mp.weixin.qq.com/debug/wxadoc/dev/index.html?t=2017118)


###代码编译

**wxapp page code**

![wxapp](https://doingsth.github.io/yongqiang.shen/img/wxapp.png))

Text.wxml

```
<view>
  <text class='text'>hello wxapp!</text>
</view>

```

Text.wxss

```
.text{
  color:#ff6633;
  margin: 20rpx;
}
```

Text.js

```
Page({})
```

**react-native code**

![rn](https://doingsth.github.io/yongqiang.shen/img/react-native.png))



transformer

```
var path = require('path')
var fs = require('fs')
var scaffold = require('scaffold-generator');

function transform(url) {
    var styleStr = fs.readFileSync(url+'.wxss', 'utf8');
    styleStr = transformer.css(styleStr)
    var tmpStr = fs.readFileSync(url+'.wxml', 'utf8');
    tmpStr = transformer.tmp(tmpStr)
    scaffold({
        data:{
            styleStr,
            tmpStr
        },
        ignore: ["**/.git/**"],
        override:true,
        noBackup:true
    }).copy(path.join(__dirname, '../wxapp/react-native-template.js'), path.join(__dirname,"./"), (err)=>{
        if(err){
            console.log(`create react-native code fail`);
        }else{
            console.info(`create react-native code  success`)
        }
    });
}

function clear(str){
    return str.replace(/<!--[^\n]*-->/g,'').replace(/\/\*[^\n]*\*\//g,'').trim()
}

var transformer = {
    css: function (str) {
        str = clear(str)
        // 解析str 生成css节点对象列表

        // 节点对象属性转化 目标节点描述

        //节点对象列表转化为str
        return str
    },
    tmp: function (str) {
        str = clear(str)
        //创建抽象语法树
        //let rootNode = UNode.createUNode(str)
        //生成目标tmp
        /*let tmp = UNode.createVueTmeplate(rootNode, {
            tag(tag) {
                return  tag
            },
            arr(arr) {
                return arr
            },
            expression(expression){
                return expression
            }
        })*/

        return str;
    }
}
transform(path.join(__dirname,'../wxapp/Text'))
```

关于代码编译根本是实现从一种DSL到另一种DSL，具体过程/手段多种多样，上述代码仅仅是证明编译的**可行性**。

###后记



下一个技术风口，**支付宝小程序**的建设欢迎你的加入！欢迎大家投递简历（**前端/后端/测试**）！

岗位优势： **新部门机会多，技术风口小程序。**

邮箱：zzz596704878@163.com

微信：![wx](https://doingsth.github.io/yongqiang.shen/img/wx.png)

沈六（沈永强）/蚂蚁金服/高级前端开发工程师/杭州













