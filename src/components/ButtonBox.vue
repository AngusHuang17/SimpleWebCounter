<template>
    <button class="button" @click="clickButton"><slot></slot></button>
</template>

<script>
export default {
  name: 'CntButton',
  methods: {
    clickButton () {
      this.$emit('cntButtonClick')
    }
  }
}
</script>

<style scoped>
.button {
  margin: 0;
  padding: 0;
  width: 98px;
  height: 40px;
  background: #2188E9;
  border: none;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.10);
  color: #FFFFFF;
  text-align: center;
  border-radius: 5px;
  opacity: 1.0;
  font-family: PingFangSC-Regular, 'PingFang SC', 'PingFang SC Regular', sans-serif;
  font-weight: normal;
  font-size: 16px;
  position: absolute;
  top: 75px;
}
</style>
