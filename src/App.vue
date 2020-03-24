<template>
  <div id="app">
    <header></header>
    <div id="input" v-if="Sig1&&!Sig2">
      <cnt-input id="hour" @getValue="getHour">时</cnt-input>
      <cnt-input id="minute" @getValue="getMinute">分</cnt-input>
      <cnt-input id="second" @getValue="getSecond">秒</cnt-input>
    </div>
    <div id="start-menu-button" v-if="Sig1&&!Sig2">
      <cnt-button id="countup" @cntButtonClick="countupFunc">开始正计时</cnt-button>
      <cnt-button id="countdown" @cntButtonClick="countdownFunc">开始倒计时</cnt-button>
    </div>
    <label id="hint" v-if="!Sig1||Sig2">{{ gHintInfo }}</label>
    <div id="count-menu-button" v-if="!Sig1||Sig2">
      <cnt-button id="restart" @cntButtonClick="restartFunc">重新再计时</cnt-button>
      <cnt-button id="clear" @cntButtonClick="clearFunc">{{ clearMsg }}</cnt-button>
    </div>
    <cnt-button id="pause" v-if="!Sig1&&Sig2" @cntButtonClick="pauseFunc">{{ pauseMsg }}</cnt-button>
    <cnt-button id="resume" v-if="Sig2&&Sig1" @cntButtonClick="resumeFunc">{{ resumeMsg }}</cnt-button>
    <div id="display-zone">
      <label id="Time">{{ gDisplayTime }}</label>
    </div>
  </div>
</template>

<script>
import CntInput from './components/InputBox'
import CntButton from './components/ButtonBox'
export default {
  name: 'App',
  data () {
    return {
      Sig1: true,
      Sig2: false,
      h: 0,
      m: 0,
      s: 0,
      gHintInfo: '正在倒计时',
      gDisplayTime: '00:00:00',
      clearMsg: '清空正计时',
      pauseMsg: '暂停正计时',
      resumeMsg: '恢复正计时',
      gTotalTime: 0,
      gClockOn: false,
      gClock: 0,
      gMode: 0,
      gCounter: 0
    }
  },
  components: {
    CntInput,
    CntButton
  },
  methods: {
    getHour (data) {
      this.h = data
    },
    getMinute (data) {
      this.m = data
    },
    getSecond (data) {
      this.s = data
    },
    timeCount () {
      if (this.gCounter < this.gTotalTime) {
        this.gCounter += 10
      } else {
        clearInterval(this.gClock)
        this.Sig1 = false
        this.Sig2 = false
        this.gClockOn = false
        if (this.gMode === 0) {
          this.gHintInfo = '正计时 ' + timeSwitch(this.gTotalTime, this.gMode, this.gTotalTime) + ' 已结束'
        } else {
          this.gHintInfo = '倒计时 ' + timeSwitch(0, this.gMode, this.gTotalTime) + ' 已结束'
        }
      }
      this.gDisplayTime = timeSwitch(this.gCounter, this.gMode, this.gTotalTime)
    },
    countupFunc () {
      this.gTotalTime = this.h * 3600 + this.m * 60 + this.s * 1
      this.gTotalTime = this.gTotalTime * 1000
      this.gCounter = 0
      this.gMode = 0
      this.gClockOn = true
      this.Sig1 = false
      this.Sig2 = true
      this.gHintInfo = '正在正计时 ' + timeSwitch(this.gTotalTime, this.gMode, this.gTotalTime)
      this.clearMsg = '清空正计时'
      this.pauseMsg = '暂停正计时'
      this.resumeMsg = '恢复正计时'
      this.gClock = setInterval(this.timeCount, 10)
    },
    countdownFunc () {
      this.gTotalTime = this.h * 3600 + this.m * 60 + this.s * 1
      this.gTotalTime = this.gTotalTime * 1000
      this.gCounter = 0
      this.gMode = 1
      this.gClockOn = true
      this.Sig1 = false
      this.Sig2 = true
      this.gHintInfo = '正在倒计时 ' + timeSwitch(0, this.gMode, this.gTotalTime)
      this.clearMsg = '清空倒计时'
      this.pauseMsg = '暂停倒计时'
      this.resumeMsg = '恢复倒计时'
      this.gClock = setInterval(this.timeCount, 10)
    },
    pauseFunc () {
      clearInterval(this.gClock)
      this.gClockOn = false
      this.Sig1 = true
      this.gHintInfo = (this.gMode === 0) ? ('暂停正计时 ' + timeSwitch(this.gTotalTime, this.gMode, this.gTotalTime))
        : ('暂停倒计时' + timeSwitch(0, this.gMode, this.gTotalTime))
    },
    resumeFunc () {
      this.gClock = setInterval(this.timeCount, 10)
      this.gClockOn = true
      this.Sig1 = false
      this.gHintInfo = (this.gMode === 0) ? ('正在正计时 ' + timeSwitch(this.gTotalTime, this.gMode, this.gTotalTime))
        : ('正在倒计时' + timeSwitch(0, this.gMode, this.gTotalTime))
    },
    restartFunc () {
      if (this.gClockOn) {
        clearInterval(this.gClock)
      }
      this.gCounter = 0
      this.gClock = setInterval(this.timeCount, 10)
      this.gClockOn = true
      this.Sig1 = false
      this.Sig2 = true
      this.gHintInfo = (this.gMode === 0) ? ('正在正计时 ' + timeSwitch(this.gTotalTime, this.gMode, this.gTotalTime))
        : ('正在倒计时' + timeSwitch(0, this.gMode, this.gTotalTime))
    },
    clearFunc () {
      if (this.gClockOn) {
        clearInterval(this.gClock)
      }
      this.gClockOn = false
      this.Sig1 = true
      this.Sig2 = false
      this.gMode = 0
      this.gCounter = 0
      this.gTotalTime = 0
      this.h = 0
      this.m = 0
      this.s = 0
      this.gDisplayTime = '00:00:00'
    },
    enterFunc () {
      if (this.Sig1 && !this.Sig2) {
        this.countupFunc()
      }
    },
    spaceFunc () {
      if (!this.Sig1 && this.Sig2) {
        this.pauseFunc()
      } else if (this.Sig1 && this.Sig2) {
        this.resumeFunc()
      } else {}
    }
  },
  mounted () {
    let that = this
    window.onkeyup = function (e) {
      if (e.keyCode === 13) {
        that.enterFunc()
      } else if (e.keyCode === 32) {
        that.spaceFunc()
      } else {}
    }
  }
}

function timeSwitch (time, gMode, gTotalTime) {
  let hour, minute, second
  if (gMode === 1) {
    time = gTotalTime - time
  }
  time = Math.floor(time / 1000)
  hour = Math.floor(time / 3600)
  minute = Math.floor((time - 3600 * hour) / 60)
  second = time - hour * 3600 - minute * 60
  if (hour < 10) {
    hour = '0' + hour
  }
  if (minute < 10) {
    minute = '0' + minute
  }
  if (second < 10) {
    second = '0' + second
  }
  let displayTime = hour + ':' + minute + ':' + second
  return displayTime
}
</script>

<style>
#app {
  margin-top: 60px;
  margin-left: 80px;
  margin-right: 30px;
  width: 1220px;
  height: 510px;
}
header {
  position: relative;
  top: 0;
  left: 0;
  width: 1220px;
  height: 70px;
  background: #97A5BC;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.10);
  border: none;
  opacity: 1.0;
}
#input {
  position: relative;
  top: -55px;
  left: 40px;
}
#minute {
  position: relative;
  top: -40px;
  left: 90px;
}
#second {
  position: relative;
  top: -80px;
  left: 180px;
}
#countup {
  left: 390px;
}
#countdown {
  left: 508px;
}
#hint {
  position: absolute;
  left: 120px;
  top: 84px;
  width: 192px;
  height: 22px;
  opacity: 1.0;
  font-family: PingFangSC-Regular, 'PingFang SC', 'PingFang SC Regular', sans-seri;
  font-weight: normal;
  font-size: 16px;
  color: #FFFFFF;
  text-align: left
}
#clear {
  left: 1004px;
  background: #DD2E1D;
}
#restart {
  left: 1122px;
  background: #FFB020;
}
#pause,#resume {
  left: 307px
}
#display-zone {
  width: 1220px;
  height: 440px;
  position: absolute;
  top: 130px;
  left: 80px;
  background: #F2F4F7
}
#Time {
  width: 960px;
  height: 200px;
  position: absolute;
  top: 127px;
  left: 131px;
  font-family: PTMono-Bold,'PT Mono',monospace;
  font-size: 200px;
  font-weight: bold;
  color: #333333;
  line-height: 200px;
}
</style>
