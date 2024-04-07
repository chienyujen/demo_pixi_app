<template>
  <div></div>
</template>

<script setup>
import {Application, Graphics} from 'pixi.js';
const app = new Application();

async function setup(){
  await app.init({
    width: window.innerWidth,
    height: window.innerHeight,
    backgroundColor: '#1099bb',
    resolution: window.devicePixelRatio || 1,
    antialias: true, // 抗鋸齒
  });
  document.body.appendChild(app.canvas);
}

async function newGraphics(){
  const rectangle = new Graphics();
  rectangle.rect(0, 0, 100, 64);
  rectangle.fill({color: '#66ccff'});
  rectangle.stroke({width:5, color:'#ffff00', alpha:1});
  // 縮放
  // rectangle.scale.set(2, 2);
  // 位移
  // rectangle.position.set(100, 100);
  // 旋轉
  // rectangle.rotation = 0.5;
  // 圖形的錨點 - 旋轉時可以用
  rectangle.pivot.set(50, 32);

  app.stage.addChild(rectangle);

  const rectangle2 = new Graphics();
  rectangle2.circle(0, 0, 50);
  rectangle2.fill({color: '#66ccff'});
  // rectangle2.position.set(300, 200);
  app.stage.addChild(rectangle2);

  const rectangle3 = new Graphics();
  rectangle3.roundRect(150, 150, 100, 64, 10);
  rectangle3.fill({color: '#66ccff'});
  // rectangle3.position.set(300, 200);
  app.stage.addChild(rectangle3);

  const rectangle4 = new Graphics();
  rectangle4.ellipse(300, 300, 100, 64);
  rectangle4.fill({color: '#66ccff'});
  // rectangle4.position.set(300, 200);
  app.stage.addChild(rectangle4);

  const polygon = new Graphics();
  polygon.poly([0,150, 0, 200, 10, 210, 20, 200, 20, 150]);
  polygon.fill({color: '#66ccff'});
  polygon.scale.set(2, 2);
  app.stage.addChild(polygon);
}

(async ()=>{
  await setup();
  await newGraphics();
})()


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

canvas {
  width: 100vw;
  height: 100hw;
  position: fixed;
  left: 0;
  top: 0;
}
</style>
