良好的用户体验和交互是一个产品提高用户粘性以及影响力的必要条件，同时也是我们前端技术人员比较专注的问题，那么css3动画就是将我们的静态页面变为更具灵动性和趣味性的一种方式，下面就让我们来了解一下它吧

animation属性是 animation-name,animation-duration,animation-timing-function,animation-delay,animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 属性的一个简写属性形式。


animation-name: 指定绑定到选择器的关键帧的名称，每个名称代表一个由@keyframes定义的动画序列
animation-duration： 指定一个动画周期的时长
animation-timing-function：定义CSS动画在每一动画周期中执行的节奏
animation-delay： 定义动画于何时开始
animation-iteration-count：定义动画在结束前运行的次数 可以是1次 无限循环。如果指定了多个值，每次播放动画时，将使用列表中的下一个值，在使用最后一个值后循环回第一个值。（1｜infinite｜等等）
animation-direction： 指示动画是否反向播放（normal | reverse | alternate | alternate-reverse）
animation-fill-mode： 设置CSS动画在执行之前和之后如何将样式应用于其目标
animation-play-state：定义一个动画是否运行或者暂停。可以通过查询它来确定动画是否正在运行。它的值可以被设置为暂停和恢复的动画的重放，恢复一个已暂停的动画，将从它开始暂停的时候，而不是从动画序列的起点开始在动画

Animation由三部分组成
1. 关键帧(keyframes) - 定义动画在不同阶段的状态
2. 动画属性(properties) - 决定动画的播放时长，播放次数，以及用何种函数式去播放动画等
3. css属性 - 就是css元素不同关键帧下的状态。

+ 关键帧
关键帧可分为多个阶段： 0%，30%，50%...
例：
```
.box {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  animation: slide 8s linear infinite;
}
@keyframes slide {
  20% {
    left: 150px;
  }
  40% {
    left: 200px;
  }
  60% {
    left: 250px;
  }
  80% {
    left: 300px;
  }
  100% {  
    left: 350px;
  }
}
```

