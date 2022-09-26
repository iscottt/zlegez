<template>
  <div id="gamePage">
    <a-row align="space-between">
      <a-button ghost style="margin-bottom: 8px" @click="doBack"> 返回</a-button>
      <a-button ghost>块数：{{ clearBlockNum }} / {{ totalBlockNum }}</a-button>
    </a-row>
    <!-- 分层选块 -->
    <a-row align="center">
      <div v-show="gameStatus > 0" class="level-board">
        <div v-for="(block, idx) in levelBlocksVal" :key="idx">
          <div
            v-if="block.status === 0"
            class="block level-block animate__animated animate__zoomIn"
            :class="{
              disabled: !isHolyLight && block.lowerThanBlocks.length > 0,
            }"
            :data-id="block.id"
            :style="{
              zIndex: 100 + block.level,
              left: block.x * widthUnit + 'px',
              top: block.y * heightUnit + 'px',
            }"
            @click="() => doClickBlock(block)"
          >
            <!-- {{ block.type }} -->
            <img :src="`https://ethanwp.oss-cn-shenzhen.aliyuncs.com/game/${block.type}`" />
          </div>
        </div>
      </div>
    </a-row>
    <!-- 随机选块 -->
    <a-row align="end" class="random-board">
      <div v-for="(randomBlock, index) in randomBlocksVal" :key="index" class="random-area">
        <div v-if="randomBlock.length > 0" :data-id="randomBlock[0].id" class="block animate__animated animate__zoomIn" @click="() => doClickBlock(randomBlock[0], index)">
          <img :src="`https://ethanwp.oss-cn-shenzhen.aliyuncs.com/game/${randomBlock[0].type}`" alt="" />
        </div>
        <!-- 隐藏 -->
        <div v-for="num in Math.max(randomBlock.length - 1, 0)" :key="num" class="block animate__animated animate__zoomIn disabled">
          <span v-if="canSeeRandom">
            <img :src="`https://ethanwp.oss-cn-shenzhen.aliyuncs.com/game/${randomBlock[num].type}`" alt="" />
          </span>
        </div>
      </div>
    </a-row>
    <!-- 槽位 -->
    <a-row v-if="slotAreaVal.length > 0" align="center" class="slot-board">
      <div v-for="(slotBlock, index) in slotAreaVal" :key="index" class="block">
        <img :class="slotBlock?.status == 2 ? 'animate__animated animate__zoomOut' : ''" v-if="slotBlock" :src="`https://ethanwp.oss-cn-shenzhen.aliyuncs.com/game/${slotBlock?.type}`" alt="" />
      </div>
    </a-row>
    <!-- 技能 -->
    <div class="skill-board">
      <a-space>
        <a-button ghost @click="doRevert">
          <template #icon><RollbackOutlined /></template>撤回
        </a-button>
        <a-button ghost @click="doRemove">
          <template #icon><VerticalAlignTopOutlined /></template>移出
        </a-button>
        <a-button ghost @click="doShuffle">
          <template #icon><RetweetOutlined /></template>洗牌
        </a-button>
        <!-- <a-button size="small" @click="doBroke">破坏</a-button>
        <a-button size="small" @click="doHolyLight">圣光</a-button>
        <a-button size="small" @click="doSeeRandom">透视</a-button> -->
      </a-space>
    </div>
    <!-- 游戏失败 -->
    <div v-if="gameStatus === 2" class="game-fail animate__animated animate__bounceIn">
      <img src="../assets/fail.png" alt="" />
      <p>游戏失败，再接再厉~</p>
      <div class="action">
        <a-button type="primary" ghost @click="reStart">重新开始</a-button>
      </div>
    </div>
    <!-- 游戏胜利 -->
    <div v-if="gameStatus === 3" class="game-success animate__animated animate__bounceIn">
      <img src="../assets/赞.png" alt="" />
      <p>恭喜，你赢啦！🎉</p>
      <div class="action">
        <a-button type="primary" ghost @click="reStart">重新开始</a-button>
      </div>
    </div>
    <div v-if="gameStatus === 2 || gameStatus === 3" class="mask animate__animated animate__fade-in"></div>
  </div>
</template>

<script setup lang="ts">
import useGame from '../core/game';
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';
import { RetweetOutlined, RollbackOutlined, VerticalAlignTopOutlined, CloseOutlined } from '@ant-design/icons-vue';

const router = useRouter();
// 重新开始
const reStart = () => {
  window.location.reload();
};
const {
  gameStatus,
  levelBlocksVal,
  randomBlocksVal,
  slotAreaVal,
  widthUnit,
  heightUnit,
  totalBlockNum,
  clearBlockNum,
  isHolyLight,
  canSeeRandom,
  doClickBlock,
  doStart,
  doShuffle,
  doBroke,
  doRemove,
  doRevert,
  doHolyLight,
  doSeeRandom,
} = useGame();

/**
 * 回上一页
 */
const doBack = () => {
  router.back();
};

onMounted(() => {
  doStart();
});
</script>

<style>
#gamePage {
  background: #391d7d;
  background-size: 200% 200%;
  padding: 16px 16px 50px;
  min-height: 100vh;
}
.level-board {
  position: relative;
  margin-top: 20px;
}

.level-block {
  position: absolute;
}

.random-board {
  margin-top: 40px;
  align-items: flex-end;
  flex-direction: column !important;
  align-items: center;
  height: 100px;
}

.random-area {
  margin-top: 8px;
  display: flex;
  align-items: center;
  justify-items: flex-end;
}

.slot-board {
  padding: 5px 10px;
  margin: 42px auto;
  width: fit-content;
  background: url('../assets/title.png') no-repeat center;
  background-size: cover;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}

.skill-board {
  text-align: center;
}

.block {
  width: 42px;
  height: 42px;
  background: white;
  border: 1px solid #fff;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
  text-align: center;
  vertical-align: top;
  display: inline-block;
  border-radius: 5px;
  transition: all 1s;
}
.block img {
  width: 42px;
  height: 42px;
}

.disabled {
  cursor: not-allowed;
  filter: brightness(60%);
}
.game-fail,
.game-success {
  width: 60vw;
  height: 320px;
  background: #fff;
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 1000;
  margin-top: -150px;
  margin-left: -30vw;
  border-radius: 10px;
  display: flex;
  align-items: center;
  flex-direction: column;
  padding: 20px;
  box-sizing: border-box;
}

.game-fail p,
.game-success p {
  font-weight: 500;
  font-size: 16px;
  color: #696969;
}

.mask {
  position: fixed;
  background: rgba(0, 0, 0, 0.4);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 999;
}

.game-fail img,
.game-success img {
  max-width: 200px;
}
</style>
