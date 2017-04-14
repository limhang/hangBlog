---
title: javascript粒子特效-烟花效果
date: 2017-04-14 11:16:19
tags: javascript粒子特效
categories: web
---

 缘由：第一次学习JavaScript的粒子特效，做个记录总结，本次更新烟花特效

<!--more-->

# 一、烟花特效
## 分析需求
主要实现点击画布，然后粒子升空爆炸的效果，分2步，第一步绘制粒子，实现升空动画爆炸动画，第二步结合物理的速度和运动规律来写

## 粒子类
```js
var Particle = function(pos, target, vel, marker, usePhysics) {

  // properties for animation
  // and colouring
  this.GRAVITY  = 0.06;
  this.alpha    = 1;
  this.easing   = Math.random() * 0.02;
  this.fade     = Math.random() * 0.1;
  this.gridX    = marker % 120;
  this.gridY    = Math.floor(marker / 120) * 12;
  this.color    = marker;

  this.pos = {
    x: pos.x || 0,
    y: pos.y || 0
  };

  this.vel = {
    x: vel.x || 0,
    y: vel.y || 0
  };

  this.lastPos = {
    x: this.pos.x,
    y: this.pos.y
  };

  this.target = {
    y: target.y || 0
  };

  this.usePhysics = usePhysics || false;

};
```

值得注意的是pos和vel属性。 这些描述了我们的粒子的位置和速度。 其余几乎都是渲染的窗饰，所以我们稍后再来看看。

首先，我们必须设置一切，您将在初始化方法中看到：

```js
  function initialize() {

    // start by measuring the viewport
    onWindowResize();

    // create a canvas for the fireworks
    mainCanvas = document.createElement('canvas');
    mainContext = mainCanvas.getContext('2d');

    // and another one for, like, an off screen buffer
    // because that's rad n all
    fireworkCanvas = document.createElement('canvas');
    fireworkContext = fireworkCanvas.getContext('2d');

    // set up the colours for the fireworks
    createFireworkPalette(12);

    // set the dimensions on the canvas
    setMainCanvasDimensions();

    // add the canvas in
    document.body.appendChild(mainCanvas);
    document.addEventListener('mouseup', createFirework, true);
    document.addEventListener('touchend', createFirework, true);

    // and now we set off
    update();
  }
```