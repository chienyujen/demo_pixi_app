<template>
  <div></div>
</template>

<script setup>
import {ShockwaveFilter} from 'pixi-filters';
import {Application, Sprite, Assets, Container, Text, DisplacementFilter } from 'pixi.js';

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

function cerateWave(waveFilter, resetTime){
    waveFilter.time += 0.01;
    if (waveFilter.time > resetTime){
      waveFilter.time = 0;
      waveFilter.center = [
      Math.random() * app.screen.width,
      Math.random() * app.screen.height,
      ]
    }
  }

async function newTexture(){
  const onyxia = await Assets.load("./textures/onyxia.jpg");
  const sprite = new Sprite(onyxia);
  sprite.label = "onyxia";
  sprite.width = app.screen.width;
  sprite.height = app.screen.height;

  const container = new Container();
  container.addChild(sprite);

  const text = new Text({
    text:"Hello World",
    style: {
        fontFamily: 'Arial',
        fontSize: 30 + Math.floor(app.screen.width * 0.05),
        fill: 0xFFFFFF,
        align: 'center',
        dropShadow: { //設定文字陰影
          color: '#aaaa00',
          blur: 4, 
          angle: Math.PI / 6,
          distance: 3,
        },       
    }    
  })

  text.x = app.screen.width / 2;
  text.y = app.screen.height * 0.75;
  text.anchor.set(0.5);
  container.addChild(text);

  app.stage.addChild(container);

  // 設定置換濾鏡
  const concrete = await Assets.load("./textures/displacement_map.png");
  concrete.source.addressMode  = 'repeat'; // 新的圖片素材 repeat 用法
  const sprite2 = new Sprite(concrete);
  sprite2.label = "displacement_map";
  sprite2.scale.set(0.8);
  const displacementFilter = new DisplacementFilter(sprite2);
  container.addChild(sprite2);

  // pixi.js v8.0 使用方式
  const shockwaveFilter = new ShockwaveFilter(
    {
      center: {
        x: Math.random() * app.screen.width,
        y: Math.random() * app.screen.height,
      },
      radius: 160, // 半徑
      wavelength: 65,// 波長
      amplitude: 100,// 震幅
      speed: 300,
      time:1,
      brightness:5,
    },
  );
  
  container.filters = [
    displacementFilter,
    shockwaveFilter,    
  ];

  app.ticker.add((time) =>{
    sprite2.x += time.deltaTime;
    sprite2.y += time.deltaTime;
    cerateWave(shockwaveFilter, 1);
  });

  container.interactive = true;
  container.on('click', (e) => {
    shockwaveFilter.center = [e.clientX, e.clientY];
    shockwaveFilter.time = 0;
  });
  
}

(async ()=>{
  await setup();
  await newTexture();
})()


function getGlobalObject() {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
}

const globalObject = getGlobalObject();
globalObject.__PIXI_APP__ = app;
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
