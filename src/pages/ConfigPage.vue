<template>
  <div id="customConfigPage">
    <h2 style="color: #fff">
      <span style="font-family: TITLE">自定义难度</span>
      <a-button ghost style="float: right" @click="doBack">返回</a-button>
    </h2>
    <a-form ref="formRef" label-align="left" :label-col="{ style: { width: '120px' } }" :model="config" @finish="handleFinish">
      <a-row>
        <a-col :span="12">
          <a-form-item label="槽容量" name="slotNum"> <a-input-number v-model:value="config.slotNum" /> </a-form-item
        ></a-col>
        <a-col :span="12">
          <a-form-item label="合成数" name="composeNum">
            <a-input-number v-model:value="config.composeNum" />
          </a-form-item>
        </a-col>

        <a-col :span="12">
          <a-form-item label="化妆品种类数" name="typeNum">
            <a-input-number v-model:value="config.typeNum" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="总层数" name="levelNum">
            <a-input-number v-model:value="config.levelNum" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="每层块数" name="levelBlockNum">
            <a-input-number v-model:value="config.levelBlockNum" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="边界收缩" name="borderStep">
            <a-input-number v-model:value="config.borderStep" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="随机区数" name="randomAreaNum">
            <a-input-number v-model:value="config.randomAreaNum" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="随机区块数" name="randomBlockNum">
            <a-input-number v-model:value="config.randomBlockNum" />
          </a-form-item>
        </a-col>
        <a-col :span="12">
          <a-form-item label="选择图标" name="animals">
            <a-select v-model:value="config.animals">
              <a-select-option value="animalsIcon">动物图标</a-select-option>
              <a-select-option value="foods">食物图标</a-select-option>
              <a-select-option value="lingshi">零食图标一</a-select-option>
              <a-select-option value="lingshiIcon">零食图标二</a-select-option>
            </a-select>
          </a-form-item>
        </a-col>
      </a-row>
      <a-form-item>
        <a-button size="large" type="primary" html-type="submit" style="margin-bottom: 20px" block> 开始 </a-button>
        <a-button size="large" block style="margin-bottom: 20px" @click="resetForm">重置 </a-button>
        <a-button size="large" block danger @click="resetConfig">还原最初配置</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>
<script setup lang="ts">
import { reactive, ref } from 'vue';
import { FormInstance } from 'ant-design-vue';
import { useRouter } from 'vue-router';
import { useGlobalStore } from '../core/globalStore';
import { IconEmun } from '../core/icon';

const formRef = ref<FormInstance>();
const router = useRouter();
const { customConfig, setGameConfig, setCustomConfig, reset } = useGlobalStore();
const initConfig = {
  randomAreaNum: 2,
  randomBlockNum: 8,
  ...customConfig,
};
(initConfig.animals as any) = 'lingshiIcon';
const config = reactive<any>(initConfig);

/**
 * 表单提交
 * @param values
 */
const handleFinish = (values: any) => {
  config.randomBlocks = new Array(values.randomAreaNum).fill(values.randomBlockNum);
  config.animals = IconEmun[config.animals];
  setGameConfig(config);
  setCustomConfig(config);
  router.push('/game');
};

const resetForm = () => {
  formRef?.value?.resetFields();
};

/**
 * 还原至初始配置
 */
const resetConfig = () => {
  reset();
  router.go(0);
};

/**
 * 回上一页
 */
const doBack = () => {
  router.back();
};
</script>
<style>
#customConfigPage {
  background: #391d7d;
  background-size: 200% 200%;
  padding: 16px 16px 50px;
  min-height: 100vh;
}
.ant-form-item-label > label {
  color: #fff !important;
}
</style>
