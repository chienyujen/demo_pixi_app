<template>
  <div></div>
</template>

<script setup>
import {Application, Sprite, Assets, Graphics} from 'pixi.js';
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
  // 建立 texture
  const texture =  await Assets.load("./textures/Icon14_17.png");

  // 建立一個 sprite
  const sprite = new Sprite(texture);

  sprite.anchor.set(0.5, 0.5)

  sprite.x = app.screen.width /2;
  sprite.y = app.screen.height /2;

  // 旋轉
  sprite.rotation = Math.PI / 4;
  // 縮放
  sprite.scale.set(5)
  // 透明度
  sprite.alpha = 0.5

  app.stage.addChild(sprite);

  // 建立動畫
  app.ticker.add((time) => {
    sprite.rotation += 0.01 * time.deltaTime;
  });

  // 建立點擊事件
  sprite.interactive = true;
  sprite.on('click', () => {
    console.log("click");
  });

  sprite.on('pointerenter', ()=>{
    sprite.alpha = 1;
  });

  sprite.on('pointerout', ()=>{
    sprite.alpha = 0.5
  });

  const rect = new Graphics();
  rect.rect(0, 0, 100, 100);
  rect.x = 100;
  rect.y = 100;
  rect.fill({color:'#ff0000'});
  app.stage.addChild(rect);

  rect.interactive = true;
  rect.on('click', (aaa)=>{
    console.log(aaa);
    console.log("click rect");
  })
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
