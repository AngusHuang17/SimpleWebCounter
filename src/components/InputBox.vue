<template>
  <div id="cnt-input">
    <input type="text" class="input-box" v-model="Value" @change="passValue" @blur="passValue" @keyup="passValue">
    <label class="unit-label"><slot></slot></label>
  </div>
</template>

<script>
export default {
  name: 'CntInput',
  el: '#cnt-input',
  methods: {
    passValue () {
      this.Value = this.Value.replace(/[^0-9]+/, '')
      if (this.Value > 59) {
        this.Value = 59
      }
      this.$emit('getValue', this.Value)
    }
  },
  props: {
    Value: {
      type: String,
      default: '0'
    }
  }

}
</script>

<style scoped>
.cnt-input {
  position: relative;
}
.input-box {
  width: 70px;
  height: 40px;
  opacity: 1.0;
  background: #FFFFFF;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.10);
  border: none;
  color: #AFAFAF;
  border-radius: 5px;
  font-family: PingFangSC-Regular, 'PingFang SC', 'PingFang SC Regular', sans-serif;
  font-size: 16px;
  text-indent: 9px;
  font-weight: normal;
  line-height: 22px;
  letter-spacing: 0;
  margin: 0, 20px, 0, 0;
  padding: 0;
}
label {
  text-align: right;
  width: 32px;
  height: 22px;
  color: #222222;
  font-size: 16px;
  font-family: PingFangSC-Regular, 'PingFang SC', 'PingFang SC Regular', sans-serif;
  font-weight: normal;
  line-height: 22px;
  letter-spacing: 0;
  opacity: 1.0;
  margin: 0;
  padding: 0;
  position: relative;
  left: -32px;
}
</style>
