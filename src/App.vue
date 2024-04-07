<template>
  <div></div>
</template>

<script setup>
import {Application, Sprite, Assets, Container} from 'pixi.js';
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

async function newTexture(){
  // 載入多個資源(texture)
  const assets = [ 
    {alias: 'bon1', src:'./textures/Icon14_17.png'},
    {alias: 'bon2', src:'./textures/bonbon.png'},
  ];
  await Assets.load(assets);
  // 建立 層級
  const container = new Container();
  // 建立一個 sprite
  const bon1 = Sprite.from('bon1');

  bon1.anchor.set(0.5, 0.5)

  bon1.x = app.screen.width /2;
  bon1.y = app.screen.height /2;
  container.addChild(bon1);
  // app.stage.addChild(bon1);

  // 建立動畫
  app.ticker.add((time) => {
    bon1.rotation += 0.01 * time.deltaTime;
  });

  const bon2 = Sprite.from('bon2');
  bon2.scale.set(0.3);
  bon2.anchor.set(0.5, 0.5)
  bon2.x = 100;
  bon2.y = 100;
  container.addChild(bon2);

  app.stage.addChild(container);
  
}

(async ()=>{
  await setup();
  await newTexture();
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
