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
    antialias: true,
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

  rectangle.position.set(100, 100);
  
  // rectangle.rotation = 0.5;

  // rectangle.pivot.set(50, 32);
  app.stage.addChild(rectangle);

  const rectangle2 = new Graphics();
  rectangle2.circle(0, 0, 50);
  rectangle2.fill({color: '#66ccff'});
  rectangle2.position.set(300, 200);
  app.stage.addChild(rectangle2);
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
