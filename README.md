# design-mode-summary
设计模式的总结



## 发布订阅模式
```
```
## 17.通信卫星---观察者模式
```
```

## 18.超级玛丽---状态模式
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

状态的变化（使用状态模式）
从上面来看，即使判断语句实现了我们的需求，其代码结构，代码的可读性以及代码的可维护性都是很糟糕的，
这样日后对新动作的添加或者原有动作的修改的成本都是很大的，甚至会影响到其他动作。所以为解决这一类问题
我们可以使用状态模式。
具体的解决问题的思路是这样的，首先创建一个状态对象，内部保存状态变量，然后内部封装好每种动作对应的状态，最后状态对象返回一个接口对象，它可以对内部的状态修改或者调用。

<!-- 创建超级玛丽状态类 -->
let MarryState = function(){
    <!-- 内部状态私有变量 -->
    let _currentState = {};
    <!-- 动作与状态方法映射 -->
    states = {
        jump:function(){
            <!-- 跳跃 -->
            console.log('jump')
        },
        move:function(){
            <!-- 移动 -->
            console.log('move')
        },
        shoot:function(){
            <!-- 射击 -->
            console.log('shoot')
        },
        squat:function(){
            <!-- 蹲下 -->
            console.log('squat')
        }
    }

    <!-- 动作控制类 -->
    let Action = {
        <!-- 改变状态方法 -->
        changeState: function(){
            <!-- 组合动作通过传递多个参数实现 -->
            let arg = arguments;
            <!-- 重置内部状态 -->
            _currentState = {};
            <!-- 如果有动作则添加动作 -->
            if(arg.length){
                <!-- 遍历动作 -->
                for(var i = 0,len = arg.length; i < len; i++){
                    <!-- 向内部状态中添加动作 -->
                    _currentState[arg[i]] = true;
                }
            }
            <!-- 返回动作控制类 -->
            return this; // 实现了链式调用
        },
        <!-- 执行动作 -->
        goes: function(){
            console.log('触发一次动作');
            <!-- 遍历内部状态保存的动作 -->
            for(let i in _currentState){
                <!-- 如果该动作存在则执行 -->
                states[i] && states[i]();
            }
            return this;
        }
    }
    <!-- 返回接口方法 change、goes -->
    return {
        change: Action.changeState,
        goes: Action.goes
    }
}
两种使用方式
对于上面状态模式封装的方法，我们通常有两种使用方式。
超级玛丽状态类顺利创建出来，当我们想使用它的时候很容易。我们有两种方式，如果你喜欢函数方式，我们可以
直接执行这个状态类，但是这只能由你自己使用，如果别人使用的时候就可能会修改状态类内部状态。
MarryState()
    .change('jump', 'shoot')  // 添加跳跃与射击动作
    .goes()                   // 执行动作
    .goes()                   // 执行动作
    .change('shoot')          // 添加射击动作
    .goes()                   // 执行动作

为了更安全我们还是实例化一下这个状态类，这样我们使用的是对状态类的一个复制品，这样无论你怎么使用，都可以放心了。
<!-- 创建一个超级玛丽 -->
let marry = new MarryState();
marry
    .change('jump', 'shoot')  // 添加跳跃与射击动作
    .goes()                   // 执行动作
    .goes()                   // 执行动作
    .change('shoot')          // 添加射击动作
    .goes()                   // 执行动作

输出结果：
<!-- 因为刚开始就执行了两次goes()，所以触发了两次动作 -->
<!-- 第一次goes -->
 触发一次动作
 jump
 shoot
<!-- 第二次goes -->
 触发一次动作
 jump
 shoot
<!-- 改变了状态 -->
<!-- 第三次goes -->
 触发一次动作
 shoot

这样我们改变了状态类的一个状态，就改变了状态对象的执行结果，看起来好像变了一个对象似的。
```

## 19.活诸葛---策略模式
```
策略模式：将定义的一组算法封装起来，使其相互之间可以替换。封装的算法具有一定独立性，不会随客户端变化而
变化。

实际需求：每年年底，公司商品展销页都要开展大促销活动，这不项目经理又在策划一次商品喷血打折大促销活动，快到年底了，商品拍卖页准备办一些活动来减轻库存压力。在圣诞节，一部分商品5折出售，一部分商品8折出售，
一部分商品9折出售，等到元旦，我们要搞个幸运反馈活动，普通用户满100返30，高级VIP用户满100返50...

技术实现思考：这么多种促销活动，实现方式要一个一个写吗？这要写到什么时候？如果要一个一个实现的话，如
下：
// 100 返 30
function return30(price){}
// 100 返 50
function return50(price){}
// 9折
function percent90(price){}
// ...
忽然间想到上次学习的状态模式，它不就是用来处理多种分支判断的么？可又一想，对于圣诞节或者元旦，当天的一
种商品只会有一种促销策略，而不用去关心其他促销状态，如果采用状态模式，那么就会为每一个商品创建一个状态
对象，这么做是不是就冗余了呢？
是的，对于一种商品的促销策略只用到一种情况，而不需要其他促销策略，此时采用策略模式会更合理。

策略模式也是一种处理多种分支判断的模式吗？
答：从结构上看，它与状态模式很像，也是在内部封装一个对象，然后通过返回的接口对象实现对内部对象的调用，
不同点就是，策略模式不需要管理状态、状态间没有依赖关系、策略之间可以相互替换、在策略对象内部保存的是相互独立的一些算法。它就像一位活诸葛，对于一件事情的处理，总有千万种计谋，每次都可以随心所欲地选择一种计
谋来表达到不同种结果。所以你也可以将你的促销策略放在活诸葛的心中，让他来帮你解决这个问题。
为实现对每种商品的策略调用。你首先要将这些算法封装在一个策略对象内，然后对每种商品的策略调用时，直接对
策略对象中的算法调用即可，而策略算法又独立地分装在策略对象内。为方便我们的管理与使用，我们需要返回一个
调用接口对象来实现对策略算法的调用。
策略对象
<!-- 价格策略对象 -->
let PriceStrategy = function(){
    <!-- 内部算法对象 -->
    let stragtegy = {
        <!-- 100返30 -->
        return30: function(price){
            <!-- parseInt 可通过~~、|等运算符替换，要注意此时price要在[] -->

        }
    }
}
```


