<template>
  <div id="indexPage">
    <div class="header">
      <img src="../assets/index_logo.png" alt="" />
      <div class="title">🎉 Duang了个Duang 🎉</div>
      <div class="desc">低配版羊了个羊小游戏，仅供消遣</div>
    </div>
    <div class="list">
      <div class="post" @click="toGamePage(easyGameConfig)">
        <div class="img" style=""></div>
        <div class="infos">
          <div class="title">简单模式</div>
          <div class="desc">难度简单，很容易过关</div>
        </div>
      </div>
      <div class="post" @click="toGamePage(middleGameConfig)">
        <div class="img"></div>
        <div class="infos">
          <div class="title">中等模式</div>
          <div class="desc">难度中等，比较容易过关</div>
        </div>
      </div>
      <div class="post" @click="toGamePage(hardGameConfig)">
        <div class="img"></div>
        <div class="infos">
          <div class="title">困难模式</div>
          <div class="desc">比较困难，不容易过关</div>
        </div>
      </div>
      <div class="post" @click="toGamePage(lunaticGameConfig)">
        <div class="img"></div>
        <div class="infos">
          <div class="title">地狱模式</div>
          <div class="desc">地狱级别难度，很难过关</div>
        </div>
      </div>
      <div class="post" @click="toGamePage(yangGameConfig)">
        <div class="img"></div>
        <div class="infos">
          <div class="title">羊了个羊模式</div>
          <div class="desc">与羊了个羊难度一直，过关凭运气</div>
        </div>
      </div>
      <div class="post" @click="() => toGamePage()">
        <div class="img"></div>
        <div class="infos">
          <div class="title">自定义 🔥</div>
          <div class="desc">自定义难度</div>
        </div>
      </div>
    </div>
    <div class="footer">
      妆了个妆 ©2022 by
      <a href="https://github.com/iscottt" target="_blank"> Scott </a>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router';
import { easyGameConfig, middleGameConfig, hardGameConfig, lunaticGameConfig, yangGameConfig } from '../core/gameConfig';
import { useGlobalStore } from '../core/globalStore';

const router = useRouter();

const { setGameConfig } = useGlobalStore();

const toGamePage = (config?: GameConfigType) => {
  if (config) {
    setGameConfig(config);
    router.push('/game');
  } else {
    router.push('/config');
  }
};
</script>

<style>
#indexPage {
  padding: 16px 16px 50px;
  min-height: 100vh;
  background: #fff;
}
.post {
  width: 100%;
  display: flex;
  gap: 1rem;
  margin-bottom: 20px;
}
.post .img {
  flex-shrink: 0;
  width: 4.5em;
  height: 4.5em;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  border-radius: 0.5em;
  box-shadow: 0 8px 10px rgba(31, 45, 61, 0.2);
  background-image: url('../assets/起飞.png');
}

.post:nth-of-type(2) .img {
  background-image: url('../assets/赞.png');
}
.post:nth-of-type(3) .img {
  background-image: url('../assets/为什么.png');
}
.post:nth-of-type(4) .img {
  background-image: url('../assets/尴尬.png');
}
.post:nth-of-type(5) .img {
  background-image: url('../assets/无奈.png');
}
.post:nth-of-type(6) .img {
  background-image: url('../assets/嗨起来.png');
}

.post .infos {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
}

.post .infos .title {
  text-decoration: none;
  color: #475669;
  text-shadow: 0 -1px #fff, 0 2px 2px rgba(31, 45, 61, 0.3);
  font-size: 16px;
  line-height: 1.6;
  transition: 0.35s;
}

.post .infos .desc {
  font-size: 13px;
  text-shadow: 0 1px #fff;
  text-align: left;
}

#indexPage {
  text-align: center;
}

.footer {
  background: #fff;
  box-shadow: 2px 0 2px rgba(31, 45, 61, 0.2);
  color: #373737;
  padding: 12px;
  text-align: center;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}

.header {
  width: calc(100% - 2.5rem);
  margin: 0 auto;
}
.header img {
  max-width: 200px;
}

.header .title {
  font-family: TITLE;
  font-size: 24px;
}

.header .desc {
  font-size: 16px;
  margin-top: 10px;
  font-weight: 500;
}

.list {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  flex-wrap: wrap;
  padding: 20px 28px;
}
</style>
