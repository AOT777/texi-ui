<template>
  <tCheckbox v-model="checkboxVal" :options='checkOptions' customColor='red'></tCheckbox>
</template>

<script setup>
  import { reactive, ref } from 'vue'
  const checkboxVal=ref([])
  const state=reactive({
    checkOptions:[
      { label: "中国", value: "zhongguo" },
      { label: "英国", value: "yingguo" },
      { label: "德国", value: "deguo" },
      { label: "法国", value: "faguo" },
    ]
  })
  const {checkOptions}=state
</script>

<style>

</style>