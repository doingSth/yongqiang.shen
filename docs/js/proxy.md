##代码君

```
function observer(data) {
    if (Object.prototype.toString.call(data) === '[object Object]') {
        Object.keys(data).forEach(key=>{
            data[key] = observer(data[key])
        })
        var obj = new Proxy(data, {
            get: function (target, key, receiver) {
                console.log(`getting :`);
                console.log(key);
                return Reflect.get(target, key, receiver);
            },
            set: function (target, key, value, receiver) {
                if (value === target[key]) {
                    return
                }
                console.log(`setting :`)
                console.log(key);
                return Reflect.set(target, key,observer(value), receiver);
            }
        });
        return obj
    }
    return data
}
```

```
//test
var testData = {
    "shop_info": {
        "branch_name": "松江店"
    },
    "discount_info": "又回来了"
}

testData=observer(testData)
console.log("update testData.shop_info.branch_name:")
testData.shop_info.branch_name =  "昌平店"
console.log("update testData.shop_info.discount_info:")
testData.shop_info.discount_info = {c:0.9}
console.log("get testData.shop_info.discount_info.c:")
testData.shop_info.discount_info.c
```

##人话

关键点：

    1.es6 Proxy 可以对整个对象进行拦截的特性 实现取值触发get  赋值触发set 
    2.es6 Reflect 具有语言内部方法 具有get->获取属性值  set->设置属性值
    
偶然得知vue3打算使用proxy实现数据观测模块，固试着写了下... 

基于proxy实现依赖收集和通知，，仅需要完成 target和key 关联 Dep对象，使得代理get函数中可以根据target和key 拿到对应的Dep对象 收集Watcher，
同理set函数获取Dep对象，通知所有的Watcher执行update

 
 
    