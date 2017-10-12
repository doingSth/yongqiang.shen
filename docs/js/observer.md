##代码君

```
var target, content;
class Dep {
    constructor() {
        this.watchs = []
    }
    addWatch() {
        target&&this.watchs.push(target)
    }
    notify() {
        this.watchs.forEach(w=>w.update())
    }
}
function observer(data) {
    if (Object.prototype.toString.call(data) === '[object Object]') {
        Object.keys(data).forEach(key=> {
            var dep = new Dep()
            var val = data[key]
            observer(data[key])
            Object.defineProperty(data, key, {
                enumerable: true,
                configurable: true,
                get: function () {
                    dep.addWatch()
                    return val
                },
                set: function (newVal) {
                    if (val === newVal) {
                        return
                    }
                    val = newVal
                    dep.notify()
                    observer(newVal)
                }
            })

        })
    }
}
class Watch {
    constructor(exp, fn) {
        this.exp = exp
        this.update = fn
        target = this
        typeof exp === 'function' ? exp.call(content) : eval(`content.${exp}`)
    }
}
```

```
//test
var testData = {
    "title": "海报标题",
    "pics": [
        {"url": "//p0.meituan.net/dprainbow/61daec840616895bc21a4a58cd68a38817000.png", "desc": "幼稚了"},
        {"url": "//p0.meituan.net/dprainbow/61daec840616895bc21a4a58cd68a38817000.png", "desc": "幼稚了"}
    ],
    "shop_info": {
        "shop_id": 9380423,
        "shop_name": "巴黎婚纱国际旗舰店",
        "branch_name": "松江店",
        "city_name": "上海"
    },
    "desc": "这是一段内容一段内容",
    "discount_info": "又回来了",
    "allow_fabulous": false, "$$id": "a0ccdf50-ae2a-11e7-88bc-89a8406df769"
}
content = testData
observer(testData)
new Watch('title',function(){
    console.log("todo update page title!")
})

new Watch('shop_info.shop_name',function(){
    console.log("todo update shop_info.shop_name!")
})

new Watch(function(){
    return this.shop_info.shop_name + "(" + this.shop_info.branch_name + ")"
},function(){
    console.log("todo update desc to dom" )
})
console.log("update title:")
testData.title = "第一次更新"
testData.title = "第二次更新"
console.log("update shop_info：")
testData.shop_info = {
    "shop_id": 1380423,
    "shop_name": "巴黎婚纱国际旗舰店",
    "branch_name": "昌平店",
    "city_name": "北京"
}
```

##图解君

![类图](https://p0.meituan.net/dprainbow/a27d7ab55f66bab82e761ec64cf3dd8459074.png)

![依赖收集流程图](https://p0.meituan.net/dprainbow/89672d67c82eed5a7804983f0c55c70380851.png)

##人话

依赖收集关键点：

    1.使用Object.defineProperty设置属性为可观测属性，即取值触发get 赋值触发set
    2.初始化Watch对象时，首先把当前对象设置为全局的target，执行观察者表达式，触发所依赖的属性的get函数(进行依赖收集)
    
依赖收集完成后，实现计算属性，数据->视图单项绑定，so easy!!!  
 
 
    