# Canimation

A simple canvas library for animation.

> Only support IE9+

## Concept
What is Animation? Animation is the transition from one state to another. Thus generally any animation could be made up of three factors: the initial state, the final state and the duration.

At anytime during the transition, the intermediate state which is also called a frame could be calculated automatically based on these factors. And Easing functions are often used to process the time-varying params to make the motion more dynamic.

动画就是状态在时间上的过渡，故任何动画由三个基本要素组成：起始状态，终止状态和时长。

在过渡期间的任何时刻的状态（又称为帧）都可以根据上述三个要素计算出来。同时，渐变函数被应用于那些随时间变化的参数，从而使动画具有更丰富的动态效果。

## Basic Usage
Move a circle straight to the right side

```js
var canvas = document.getElementById('canvas')
var cnm = new Canimation(canvas, window.innerWidth, window.innerHeight);
cnm.createAnimation({
  circle: {
    from: {
      x: 200,
      y: 300,
      r: 15
    },
    to: {
      x: 500
    },
    duration: 3000
  }
});
```

Let the circle scale up at the same time
```js
cnm.createAnimation({
  circle: {
    from: {
      x: 200,
      y: 300,
      r: 15
    },
    to: {
      x: 500,
      r: 30
    },
    duration: 3000
  }
});
```

Move more than one circle to anywhere
```js
cnm.createAnimation({
  circle: {
    from: [{
      x: 100,
      y: 100,
      r: 10
    }, {
      x: 200,
      y: 200,
      r: 20
    }, {
      x: 300,
      y: 300,
      r: 30
    }],
    to: [{
      x: 500,
    }, {
      y: 500,
    }, {
      x: 500,
      y: 500
    }],
    duration: 3000
  }
});
```

## Examples

<a href="https://bison1994.github.io/kidney/canimation/example/example1.html" target="_blank">example1</a>
<br>
<a href="https://bison1994.github.io/kidney/canimation/example/example2.html" target="_blank">example2</a>

## API

### 基本用法

```js
var canvas = document.getElementById('canvas');
var w = 1000;
var h = 500;
var cnm = new Canimation(canvas, w, h);
/**
 * 创建一组动画，并立即执行
 * 在何时创建动画可自由决定
 * 可以多次调用，从而创建多组动画
 * 
 * @options { Object } 动画包含的元素及其 action
 * @loop { Boolean } 是否无限循环，默认为 false
 */
cnm.createAnimation(options, loop);
```

- options 的每个属性表示一个或一组 canvas 元素，目前支持粒子（circle）、椭圆曲线（arc）、文本（text）
- 属性值为一个对象，表示一个基本动画，称为 action，它具有以下属性：

```js
{
  from: {},         // 元素的初始状态参数
  to: {},           // 元素的终止状态参数，坐标参数沿直线轨迹变化
  arcTo: {},        // 元素的终止状态参数，坐标参数沿椭圆轨迹变化
  duration: 1000,   // 过渡时长
  easing: 'linear'  // 过渡函数
}
```

- 如果同一个 action 中有多个同类元素，则将 from 设为数组，相应的 to 或 arcTo 也必须是对应的数组
```js
{
  from: [{
    x: 0,
    y: 0
  }, {
    x: 10,
    y: 10
  }],
  to: [{
    x: 100,
    y: 100
  }, {
    x: 200,
    y: 200
  }]
}
```

- 一个 action 无论有多少元素，都共享同样的 duration 和 easing 函数，如果要生成不同 duration 或 easing 的 action，则应创造多个 action

```js
{
  from: [],
  to: [],
  duration: 1000
},
{
  from: [],
  to: [],
  duration: 2000
}
```
- 只有数值型的参数才可以用于动效

### 粒子元素的参数
```js
article: {
  from: {
    x: 0,                   // 圆心横坐标
    y: 0,                   // 圆心纵坐标
    r: 10,                  // 半径
    fill: '#000'            // 填充色
  }
}
```

### 椭圆曲线的参数
```js
arc: {
  from: {
    x: 500,                 // 圆心横坐标
    y: 500,                 // 圆心纵坐标
    r: 200,                 // 半径
    s: 0,                   // 起始角度
    e: 2,                   // 终止角度
    c: false,               // 旋转方向
    width: 5,               // 曲线宽度
    fill: 'transparent',    // 填充色
    stroke: '#000'          // 曲线颜色
  }
}
```

### 文本元素参数
数字型文本可以设置渐增或渐减的动效
```js
text: {
  from: {
    text: 0,                // 文本
    x: 0,                   // 横坐标
    y: 0,                   // 纵坐标
    fill: '#000',           // 文本颜色
    font: '14px Arial'      // 文本大小和字体
  }
}
```

### to 属性
from 中所有数值型的属性，均可以作为 to 的属性，注意保持结构上一一对应

### arcTo 属性
arcTo 可设置的属性和 to 完全一样，只不过 arcTo 必须额外设置椭圆轨迹参数<br>
确定一个椭圆，需要四个参数：横坐标、纵坐标、长半径、短半径<br>
确定元素停止在轨迹上的哪个位置，还需一个角度参数<br>
为避免与 circle 参数属性名重合，椭圆轨迹属性名均添加下划线以示区分

```js
arcTo: {
  _x: 500,      // 圆心横坐标
  _y: 505,      // 圆心纵坐标
  _R: 200,      // 长半轴
  _r: 100,      // 短半轴
  _e: 1         // 终止位置的角度
}
```

注意，要确保元素沿椭圆轨迹运行，元素初始位置必须设置在椭圆轨迹之上

## 其它
关于 arc 角度，[参考](http://www.w3school.com.cn/tags/canvas_arc.asp)