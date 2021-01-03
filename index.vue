<template>
  <div class="ser_home">
    <div style="color:red" @click="startCountDown()">开始{{startCount}}s</div>
    <div class="countDown">倒计时{{durationTag}}s</div>
    <ul class="red_packet" id="red_packet">
      <li
        v-for="(item1, index1) in redPacketParams"
        :key="item1.scales"
        @webkitAnimationEnd="removeDom(redPacketParams,index1)"
        @touchstart="tapMethod"
      >
        <div
          class="box"
          id="box"
          :style="{ left: item1.left, animationDuration: item1.durTime, webkitAnimationDuration: item1.durTime}"
          :class="item1.cls"
          :data-index="index1"
        >
          <a href="javascript:;">
            <i :style="{ transform: item1.transforms, webkitTransform: item1.transforms}"></i>
          </a>
        </div>
      </li>
      <li
        v-for="(item, index) in fuParams"
        :key="index+item"
        @webkitAnimationEnd="removeDom(fuParams,index)"
        @click="tapMethod"
      >
        <div
          class="box"
          :style="{ left: item.left, animationDuration: item.durTime, webkitAnimationDuration: item.durTime}"
          :class="item.cls"
          :data-index="index"
        >
          <a href="javascript:;">
            <i class="fu" :style="{ transform: item.transforms, webkitTransform: item.transforms}"></i>
          </a>
        </div>
      </li>
      <li
        v-for="(item2, index) in beastParams"
        :key="index"
        @webkitAnimationEnd="removeDom(beastParams,index)"
        @click="tapMethod"
      >
        <div
          class="box"
          :style="{ left: item2.left, animationDuration: item2.durTime, webkitAnimationDuration: item2.durTime}"
          :class="item2.cls"
          :data-index="index"
        >
          <a href="javascript:;">
            <i class="beast" :style="{ transform: item2.transforms, webkitTransform: item2.transforms}"></i>
          </a>
        </div>
      </li>
      <li
        v-for="(data, index) in firecrackersParams"
        :key="'haha'+index"
        @webkitAnimationEnd="removeDom(firecrackersParams,index)"
        @click="tapMethod"
      >
        <div
          class="box"
          :style="{ left: data.left, animationDuration: data.durTime, webkitAnimationDuration: data.durTime}"
          :class="data.cls"
          :data-index="index"
        >
          <a href="javascript:;">
            <i class="firecrackers" :style="{ transform: data.transforms, webkitTransform: data.transforms}"></i>
          </a>
        </div>
      </li>
    </ul>
  </div>
</template>


<script>
export default {
  data() {
    return {
      redPacketParams: [],
      fuParams: [],
      beastParams:[],
      firecrackersParams:[],
      timer: null,
      timer1: null,
      duration: 10000, // 定义时间
      durationTag: '', 
      num: 0,
      fuNum: 0,
      beastNum:0,
      firecrackersNum:0,
      now: 0,
      countDown:'',
      startCount:6,
      Level:0
    };
  },
  mounted() {
    
    // this.start();
  },
  computed:{
    // countDown(){
    //   setTimeout(() => {
    //     return (this.duration - 1000) / 1000
    //   }, 1000);
    // }
  },
  methods: {
      startCountDown(){
          let timer1 = setInterval(() => {
          if(this.startCount == 0){
              this.Level += 1;
              this.startGame();
              clearInterval(timer1);
              return;
          }
        this.startCount -= 1;
      }, 1000);
      },
    startGame(Level) {
        if(this.Level == 1){
            this.duration = 25000;
            this.durationTag = 25;
        }else if(this.Level == 2){
            this.duration = 20000;
            this.durationTag = 20;
        }else{
            this.duration = 15000;
            this.durationTag = 15;
        }
      let timer2 = setInterval(() => {
          if(this.durationTag == 0){
              clearInterval(timer2);
              this.startCount = 6;
              if(this.Level < 3){
                  this.startCountDown();
              }
              return;
          }
        this.durationTag -= 1;
      }, 1000);
      this.now = new Date().getTime();
      //creatFuc(红包下落速度,元素数组,个数,红包峰值,下落间隔时间)
      let redInterval = 0;
      let redDuration = '';
      if(this.Level == 1){
            redInterval = 400;
            redDuration = 3;
        }else if(this.Level == 2){
            redInterval = 350;
            redDuration = 2;
        }else{
            redInterval = 260;
            redDuration = 1;
        }
      this.creatFuc(redDuration,this.redPacketParams,this.num,56,redInterval);
      setTimeout(() => {
          this.creatFuc(2,this.fuParams,this.fuNum,2,5000);
      }, 3000);
      setTimeout(() => {
          this.creatFuc(1,this.beastParams,this.beastNum,3,4000);
      }, 5000);
      setTimeout(() => {
          this.creatFuc(1,this.firecrackersParams,this.firecrackersNum,3,4000);
      }, 4000);
    },
    /**
     * 开启动画
     */
    startRedPacket() {
      let win =
        document.documentElement.clientWidth || document.body.clientWidth;
      let left = parseInt(Math.random() * (win - 50) + 0);
      let time = null;

      let rotate = parseInt(Math.random() * (45 - -45) - 45) + "deg"; // 旋转角度
      let scales = (Math.random() * (12 - 8 + 1) + 8) * 0.1; // 图片尺寸
      let durTime = Math.random() * (2.5 - 1.2 + 1) + 1.2 + "s"; // 时间  1.2和1.2这个数值保持一样
      //   console.log(durTime);
      this.redPacketParams.push({
        left: left + "px",
        cls: "move_1",
        transforms: "rotate(" + rotate + ") scale(" + scales + ")",
        durTime: durTime
      });
      this.num += 1;
      if (this.num == 56) {
        clearTimeout(time);
        return;
      }
      //   setTimeout(() => {
      //     // 多少时间结束
      //     clearTimeout(time);
      //     return;
      //   }, this.duration);

      time = setTimeout(() => {
        if (new Date().getTime() - this.now > this.duration) {
          clearTimeout(time);
          return;
        }
        this.startRedPacket();
      }, 400);
    },
    startRedPacket2() {
      let win =
        document.documentElement.clientWidth || document.body.clientWidth;
      let left = parseInt(Math.random() * (win - 50) + 0);

      let rotate = parseInt(Math.random() * (45 - -45) - 45) + "deg"; // 旋转角度
      let scales = (Math.random() * (12 - 8 + 1) + 8) * 0.1; // 图片尺寸
      let durTime = Math.random() * (2.5 - 1.2 + 1) + 1.2 + "s"; // 时间  1.2和1.2这个数值保持一样
      //   console.log(durTime);
      this.fuParams.push({
        left: left + "px",
        cls: "move_1",
        transforms: "rotate(" + rotate + ") scale(" + scales + ")",
        durTime: durTime
      });
      this.fuNum += 1;
      if (this.fuNum == 2) {
        clearTimeout(this.timer1);
        return;
      }
      setTimeout(() => {
        // 多少时间结束
        clearTimeout(this.timer1);
        return;
      }, this.duration);

      this.timer1 = setTimeout(() => {
        this.startRedPacket2();
      }, 9000);
    },
    //creatFuc(红包下落速度,元素数组,个数,红包峰值,下落间隔时间)
    creatFuc(durTime, arr, num,max,intervalTime) {
      let win =
        document.documentElement.clientWidth || document.body.clientWidth;
      let left = parseInt(Math.random() * (win - 50) + 0);
      let time = null;

      let rotate = parseInt(Math.random() * (45 - -45) - 45) + "deg"; // 旋转角度
      let scales = (Math.random() * (12 - 8 + 1) + 8) * 0.1; // 图片尺寸
      let durTimeNew = Math.random() * (durTime - 1.2 + 1) + 1 + "s"; // 时间  1.2和1.2这个数值保持一样
      arr.push({
        left: left + "px",
        cls: "move_1",
        transforms: "rotate(" + rotate + ") scale(" + scales + ")",
        durTime: durTimeNew
      });
      num += 1;
      if (num == max) {
        clearTimeout(time);
        return;
      }
      time = setTimeout(() => {
        if (new Date().getTime() - this.now > this.duration) {
          clearTimeout(time);
          return;
        }
        this.creatFuc(durTime, arr, num,max,intervalTime);
      }, intervalTime);
    },
    /**
     * 回收dom节点
     */
    removeDom(arr, index) {
      console.log(index);
      //   arr.splice(index,1);
    },
    tapMethod(e) {
      let target = e.currentTarget;
      document.querySelector("#red_packet").removeChild(target);
      console.log("haha");
    }
  }
};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ser_home {
  width: 100%;
  height: 100vh;
}
.red_packet {
  display: block;
  position: relative;
  overflow: hidden;
  width: 100%;
  height: 100%;
}
i {
  width: 48px;
  height: 69px;
  display: block;
  background: url("../../images/one.jpg") no-repeat;
  background-size: 100% 100%;
}
i.fu {
  width: 48px;
  height: 69px;
  display: block;
  background: url("../../images/tow.jpg") no-repeat;
  background-size: 100% 100%;
}
i.beast {
  width: 48px;
  height: 69px;
  display: block;
  background: url("../../images/demo.png") no-repeat;
  background-size: 100% 100%;
}
i.firecrackers {
  width: 48px;
  height: 69px;
  display: block;
  background: url("../../images/4-4.png") no-repeat;
  background-size: 100% 100%;
}
.box {
  position: absolute;
  animation: all 0.1s linear;
  top: -100px;
  z-index: 10;
}
div.move_1 {
  -webkit-animation: aim_move 10s linear 1 forwards;
  animation: aim_move 10s linear 1 forwards;
}
a {
  display: block;
}
.countDown{
    position: absolute;
    left: 0;
    top: 20%;
    width: 30px;
    color: red;
}
@keyframes aim_move {
  0% {
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  100% {
    -webkit-transform: translateY(120vh);
    transform: translateY(120vh);
  }
}
</style>