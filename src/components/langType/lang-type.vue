<template>
  <span id="lang-type"><slot></slot></span>
</template>

<script>
export default {
  name: "lang-type",
  data() {
    return {};
  }
};
</script>

<style lang="less" scoped>
@import "../../style/mixin";
#lang-type {
  display: inline-block;
  border: 1px solid #74abcc;
  color: #74abcc;
  text-align: center;
  background: #e6f6ff;
  font-size: @smallSize;
  padding: 0px 2px 0;
  vertical-align: text-top;
  .borderRadius(2px);
}
</style>