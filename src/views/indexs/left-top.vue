<!--
 * @Author: daidai
 * @Date: 2022-02-28 16:16:42
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-07-20 17:57:11
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\left-center.vue
-->
<template>
  <ul class="user_Overview flex" v-if="pageflag">
    <li class="user_Overview-item" style="color: #00fdfa">
      <div class="user_Overview_nums allnum ">
        {{ sysData.total }}
        <!--                <dv-digital-flop :config="config" style="width:100%;height:100%;" />-->
      </div>
      <p>系统总数</p>
    </li>
    <li class="user_Overview-item" style="color: #07f7a8">
      <div class="user_Overview_nums online">
        {{ sysData.iot }}
        <!--                <dv-digital-flop :config="onlineconfig" style="width:100%;height:100%;" />-->
      </div>
      <p>4G版</p>
    </li>
    <li class="user_Overview-item" style="color: #e3b337">
      <div class="user_Overview_nums offline">
        {{ sysData.wifi }}
        <!--                <dv-digital-flop :config="offlineconfig" style="width:100%;height:100%;" />-->

      </div>
      <p>WiFi版</p>
    </li>
    <!--        <li class="user_Overview-item" style="color: #f5023d">-->
    <!--            <div class="user_Overview_nums laramnum">-->
    <!--                <dv-digital-flop :config="laramnumconfig" style="width:100%;height:100%;" />-->
    <!--            </div>-->
    <!--            <p>告警次数</p>-->
    <!--        </li>-->
  </ul>
  <Reacquire v-else @onclick="getData" line-height="200px">
    重新获取
  </Reacquire>
</template>

<script>
import {currentGET} from 'api/modules'

let style = {
  fontSize: 24
}
export default {
  data() {
    return {
      sysData: {},
      options: {},
      pageflag: true,
      timer: null,
    };
  },
  filters: {
    numsFilter(msg) {
      return msg || 0;
    },
  },
  created() {
    this.getData()
  },
  mounted() {
  },
  beforeDestroy() {
    this.clearData()

  },
  methods: {
    clearData() {
      if (this.timer) {
        clearInterval(this.timer)
        this.timer = null
      }
    },
    getData() {
      this.pageflag = true;
      this.request.get("/hospital/sysModel").then(res => {
        console.log("系统总览",res.data[0])
       
        this.sysData=res.data[0]
        
      });
      // this.sysData = {
      //   total: 300,
      //   iot: 200,
      //   wifi: 100
      // }
      //轮询
      this.switper()
     
    },
    //轮询
    switper() {
      if (this.timer) {
        return
      }
      let looper = (a) => {
        this.getData()
      };
      this.timer = setInterval(looper, this.$store.state.setting.echartsAutoTime);
    },
  },
};
</script>
<style lang='scss' scoped>
.user_Overview {
  li {
    flex: 1;

    p {
      text-align: center;
      height: 16px;
      font-size: 16px;
    }

    .user_Overview_nums {
      width: 100px;
      height: 100px;
      text-align: center;
      line-height: 100px;
      font-size: 22px;
      margin: 50px auto 30px;
      background-size: cover;
      background-position: center center;
      position: relative;

      &::before {
        content: '';
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
      }

      &.bgdonghua::before {
        animation: rotating 14s linear infinite;
      }
    }

    .allnum {

      // background-image: url("../../assets/img/left_top_lan.png");
      &::before {
        background-image: url("../../assets/img/left_top_lan.png");

      }
    }

    .online {
      &::before {
        background-image: url("../../assets/img/left_top_lv.png");

      }
    }

    .offline {
      &::before {
        background-image: url("../../assets/img/left_top_huang.png");

      }
    }

    .laramnum {
      &::before {
        background-image: url("../../assets/img/left_top_hong.png");

      }
    }
  }
}
</style>
