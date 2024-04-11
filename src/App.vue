<template>
  <div></div>
</template>

<script setup>
// import {ShockwaveFilter} from 'pixi-filters';
import {Application, Sprite, Assets, Container, Texture, Rectangle, AnimatedSprite, TilingSprite, Text } from 'pixi.js';

const app = new Application();
let isGaming = false;
let isGameOver = false;


async function setup(){
  await app.init({
    width: window.innerWidth,
    height: window.innerHeight,
    backgroundColor: '#ffffff',
    resolution: window.devicePixelRatio || 1,
    antialias: true, // 抗鋸齒
  });
  document.body.appendChild(app.canvas);
}

async function newGame(){
  const container = new Container();
  app.stage.addChild(container);
  
  // 引入整個素材
  const base = await Assets.load([
    {alias:"base", src:"./textures/dino.png"},
  ]);

  // 建立恐龍
  const dinoTexture = new Texture({
    source:base.base,
    label:"dino",
    frame: new Rectangle(75, 0, 88, 100),
  });

  const dino = new Sprite(dinoTexture);
  dino.visible = false;
  container.addChild(dino);

  // 恐龍跑步動畫
  const runTextures = [];
  for(let i = 0; i < 2; i++){
    runTextures.push(
      new Texture({
        source:base.base,
        label: "dino_run_"+i,
        frame: new Rectangle(1680 + (2 + i) * 88, 0, 82, 100),
      })
    );
  }

  const runAnimation = new AnimatedSprite(runTextures);
  runAnimation.animationSpeed = 0.1;
  runAnimation.play();
  runAnimation.visible = false
  container.addChild(runAnimation);

  // 恐龍跳躍
  const jumpTexture = new Texture({
    source:base.base,
    label:"jump",
    frame: new Rectangle(1680, 0, 82, 100),
  });

  const jumpSprite = new Sprite(jumpTexture);
  jumpSprite.y = window.innerHeight - 50 - 100;
  jumpSprite.x = 60;
  jumpSprite.visible = false;
  container.addChild(jumpSprite);

  // 建立地面物件
  const groundTexture = new Texture({
    source:base.base,
    label:"ground",
    frame: new Rectangle(50, 100, 2300, 30),
  });

  const groundSprite = new TilingSprite(groundTexture);
  groundSprite.width = window.innerWidth;
  groundSprite.height = 30;
  // 設定地面素材在畫面上的位置
  groundSprite.position.set(0, window.innerHeight - 50);
  container.addChild(groundSprite);


  // 建立仙人掌
  const cactusTexture = new Texture({
    source:base.base,
    label:"cactus",
    frame: new Rectangle(515, 0, 30, 60),
  });

  const cactusSprite = new Sprite(cactusTexture);
  cactusSprite.x = window.innerWidth / 2;
  cactusSprite.y = window.innerHeight - 50 - 60;
  cactusSprite.visible = false;
  container.addChild(cactusSprite);

  // 建立開始的按鈕文字
  let startText = new Text({
    text: "Start Game",
    style: {
      fontFamily: 'Arial',
      fontSize: 24,
      fill: 0x000000,
      align: 'center',
    }
  });
  startText.x = window.innerWidth/2;
  startText.y = window.innerHeight/2;
  startText.anchor.set(0.5)
  container.addChild(startText);

  function playGame(){
    if (isGaming){
      console.log("已經在遊戲中");
      return;
    }
    isGaming = true;
    console.log("開始遊戲");

    // 開始跑步動畫
    runAnimation.visible = true;
    runAnimation.x = 60;
    runAnimation.y = window.innerHeight - 50 - 100;

    cactusSprite.visible = true;
  }

  // 遊戲得分
  let score = 0;
  console.log(score);
  // 跳躍速度
  let jumpVelocity = 20;
  console.log(jumpVelocity);
  // 初始化重力
  let gravity = 1;
  console.log(gravity);

  function gameOver(){
    isGameOver = true;
    isGaming = false;
    
  }
  
  startText.interactive = true;
  startText.on('click', () =>{
    playGame();
  });

  

  // 遊戲循環
  app.ticker.add(() => {
    if(isGameOver){
      return;
    }
    if(isGaming){
      //地面移動
      groundSprite.tilePosition.x -= 10;
      //仙人掌移動
      cactusSprite.x -=10;
      if ( cactusSprite.x < -30) {
        cactusSprite.x = window.innerWidth;
        score++;
      }
    }

    // 跳躍的動畫
    if (jumpSprite.visible){
      jumpVelocity -= gravity;
      jumpSprite.y -= jumpVelocity;
      if (jumpSprite.y > window.innerHeight - 50 - 100){
        jumpSprite.y = window.innerHeight - 50 - 100;
        jumpVelocity = 20;
        jumpSprite.visible = false;
        runAnimation.visible = true;
      }
    }

    // 檢查碰撞
    if(
      jumpSprite.y > cactusSprite.y - 60 && 
      cactusSprite.x < jumpSprite.x +60 && 
      cactusSprite.x > jumpSprite.x-60
    ){
      console.log("碰撞了");
      gameOver();
      startText.visible = false;

      let overText = new Text({
        text: "Game Over, 總得分:"+score,
        style: {
          fontFamily: 'Arial',
          fontSize: 24,
          fill: 0x000000,
          align: 'center',
        },
      });
      overText.x = window.innerWidth/2;
      overText.y = window.innerHeight/2;
      overText.anchor.set(0.5)
      container.addChild(overText);

      overText.interactive = true;
      overText.on('click', () => {
        location.reload();
      });
    }
  });

  window.addEventListener("keydown",(e) => { 
    if(e.code === "Space"){
      console.log("跳躍");
      runAnimation.visible = false;
      jumpSprite.visible = true;
      jumpVelocity = 20;
    }
  });

}

(async ()=>{
  await setup();
  await newGame();
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
