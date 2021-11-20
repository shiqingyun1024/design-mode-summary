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

那么我们想调用第三种结果，我们就可以按照下面这种方式来实现。
<!-- 展示结果3 -->
ResultState.show(3)
上面的简单案例展示了状态模式的基本雏形。
对于状态模式，主要的目的就是将条件判断的不同结果转化为状态对象的内部状态，既然是状态对象的内部状态，
所以一般作为状态对象内部的私有变量，然后提供一个能够调用状态对象内部状态的接口方法对象。
这样当我们需要增加、修改、调用、删除某种状态方法时就会很容易，也方便了我们对状态对象中内部状态的管理。

超级玛丽的案例：
我们小时候玩的超级玛丽游戏，玛丽吃蘑菇时，他会跳起；玛丽想避免被前面的乌龟吃掉，他会开枪将其打死；
前方飞过炮弹，玛丽要蹲下躲避；时间不够了，玛丽要加速奔跑.....等等，他会有好多动作。跳跃，开枪，蹲下，
奔跑等，这些都是一个一个状态，如果我们用if或者switch条件判断语句写的代码一听就不靠谱，而且也是很难维
护的，因为增加或者删除一个状态需要改动的地方太多了。此时使用状态模式就再好不过了。对与玛丽，有的时候它
需要跳跃开枪，有的时候需要蹲下开枪，有的时候需要奔跑开枪等，如果这些组合状态用if或者switch分支判断去
是实现，无形中增加的成本是无法想象的。举例如下：

<!-- 单动作条件判断，每增加一个动作就需要添加一个判断 -->
let lastAction = '';
function changeMarray(action){
    if(action == 'jump'){
        <!-- 跳跃动作 -->
    }else if(action == 'move'){
        <!-- 移动动作 -->
    }else{
        <!-- 默认动作 -->
    }
    lastAction = action;
}

<!-- 复合动作对条件判断的开销是翻倍的 -->
let lastAction1 = '';
let lastAction2 = '';
function changeMarry(action1,action2){
    if(action1 == 'shoot'){
        <!-- 射击 -->
    }else if(action1 == 'jump'){
        <!-- 跳跃 -->
    }else if(action1 == 'move' && action2 == 'shoot'){
        <!-- 移动中射击 -->
    }else if(action1 == 'jump' && action2 == 'shoot'){
        <!-- 跳跃中射击 -->
    }
    <!-- 保留上一个动作 -->
    lastAction1 = action1 || '';
    lastAction2 = action2 || '';
}

状态的变化


```

## 策略模式
```
```


