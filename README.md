# design-mode-summary
设计模式的总结

## 观察者模式
```
```

## 发布订阅模式
```
```

## 状态模式
```
状态模式：当一个对象的内部状态发生改变时，会导致其行为的改变，这看起来像是改变了对象。

状态对象的实现
简单案例：
let ResultState = function(){
    <!-- 判断结果保存在内部状态中 -->
    let States = {
        <!-- 每种状态作为一种独立方法保存 -->
        state0:function(){
            <!-- 处理结果 0 -->
            console.log('这是第一种情况')
        },
        state1:function(){
            <!-- 处理结果 1 -->
            console.log('这是第一种情况')
        },
        state2:function(){
            <!-- 处理结果 2 -->
            console.log('这是第一种情况')
        },
        state3:function(){
            <!-- 处理结果 3 -->
            console.log('这是第一种情况')
        }
    }
    <!-- 获取某一种状态并执行其对应的方法 -->
    function show(result){
        States['state'+result] && States['state'+result]();
    }
    return {
        <!-- 返回调用状态的方法接口 -->
        show:show
    }
}();

```

## 策略模式
```
```


