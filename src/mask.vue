<template>
  <div></div>
</template>

<script setup>
import {Application, Sprite, Assets, Container, Text, Graphics} from 'pixi.js';
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

async function newTextlab(){
  const container = new Container();
  const assets1 = [ 
    {alias: 'anivia', src:'./textures/Anivia.jpg'},
  ];
  await Assets.load(assets1);

  const text = new Text({
    text: 'Hello World',
    style: {
      fontFamily: 'Arial',
         fontSize: 48,
         fill: 0xff1010,
         align: 'center',
    },
  })

  text.x = app.screen.width / 2;
  text.y = app.screen.height / 2;
  // text.anchor.set(0.5);
  
  container.addChild(text);

  const sircle = new Graphics();
  sircle.circle(app.screen.width/2, app.screen.height/2, 100); 
  sircle.fill({color:"#0000ff"});

  const bunny = Sprite.from("anivia");
  bunny.width = app.screen.width;
  bunny.height = app.screen.height;
  bunny.mask = sircle;
  container.addChild(bunny);

  app.stage.addChild(container);
}

(async ()=>{
  await setup();
  await newTextlab();
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
