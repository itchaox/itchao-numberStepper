<!--
 * @Version    : v1.00
 * @Author     : itchaox
 * @Date       : 2023-09-26 15:10
 * @LastAuthor : itchaox
 * @LastTime   : 2023-11-18 19:22
 * @desc       : 
-->
<script setup>
  import { ref, watch } from 'vue';
  import { bitable } from '@lark-base-open/js-sdk';

  const base = bitable.base;

  const fieldId = ref();
  const recordId = ref();

  const step = ref(1);
  const currentValue = ref(10);

  bitable.base.onSelectionChange(async (event) => {
    // 获取点击的字段id和记录id
    fieldId.value = event.data.fieldId;
    recordId.value = event.data.recordId;

    const table = await base.getActiveTable();
    if (fieldId.value && recordId.value) {
      // 修改当前数据
      currentValue.value = await table.getCellValue(fieldId.value, recordId.value);
    }
  });

  watch(currentValue, async (newValue, oldValue) => {
    const table = await base.getActiveTable();
    if (fieldId.value && recordId.value) {
      await table.setCellValue(fieldId.value, recordId.value, newValue);
    }
  });
</script>

<template>
  <div>
    <div class="title">【请选择<span class="high">数字类型</span>的单元格】</div>
    <div class="step">
      <div class="step-tip">请设置步进值:</div>
      <el-input-number
        v-model="step"
        :min="1"
        size="small"
      />
    </div>
    <div class="tip">步进值: 每次增加或减少的固定数值</div>

    <div class="current">
      <div class="current-tip"><span class="high">当前选中</span>单元格的数据</div>
      <el-input-number
        v-model="currentValue"
        :step="step"
      />
    </div>
  </div>
</template>

<style scoped>
  .title {
    margin-bottom: 14px;
  }

  .high {
    color: dodgerblue;
  }

  .step {
    display: flex;
    align-items: center;
    margin-bottom: 5px;
    .step-tip {
      font-size: 14px;
      color: #1f2329;
      margin-right: 8px;
    }
  }

  .tip {
    font-size: 12px;
    margin-bottom: 34px;
    color: #87959e;
  }

  .current {
    .current-tip {
      margin-bottom: 12px;
    }
  }
</style>
