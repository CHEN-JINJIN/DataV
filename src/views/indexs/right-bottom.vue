<!--
 * @Author: daidai
 * @Date: 2022-03-01 15:27:58
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-05-07 11:24:14
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\right-center.vue
-->
<template>
  <div v-if="pageflag" class="right_center_wrap beautify-scroll-def" :class="{ 'overflow-y-auto': !sbtxSwiperFlag }">
    <component :is="components" :data="list" :class-option="defaultOption">
      
      <ul class="right_center ">
        <li class="right_center_item" :class="{ 'pointer': sshFlag && item.ssh_port!=null }" v-for="(item, i) in list" :key="i" @click="toUrl(item)">
          <span class="orderNum" style="width:12px; text-align:right;">{{ i + 1 }}</span>
          <div class="inner_right">
            <div class="dibu"></div>

            <div style="display: flex">

              <div class="info" style="flex: 1;text-align: left;font-size:17px">
<!--                <span class="labels"> 医院编号：</span>-->
                 {{ item.hospital_no }}
              </div>
              <div class="info" style="flex: 5;font-size:17px;white-space: nowrap;">
<!--                <span class="labels">时间：</span>-->
                 {{ item.hospital_name }}
              </div>
              <div class="info" style="flex: 1;text-align:right;font-size:17px;" >
                <!--                <span class="labels"> 医院编号：</span>-->
                <i style="color: #00C0A3; font-size: 17px;" v-if="item.ssh_status ==1" class="el-icon-s-promotion"></i>
                <i style="color: #FF9671; font-size: 17px;" v-if="item.ssh_status ==0" class="el-icon-loading"></i>
              </div>

            </div>

          </div>
        </li>
      </ul>
    </component>
  </div>
  <Reacquire v-else @onclick="getData" style="line-height:200px" />

</template>

<script>
import { currentGET } from 'api/modules'
import vueSeamlessScroll from 'vue-seamless-scroll'  // vue2引入方式
import Kong from '../../components/kong.vue'
export default {
  components: { vueSeamlessScroll, Kong },

  data() {
    return {
      list: [],
      timer: null,
      pageflag: true,
      defaultOption: {
        ...this.$store.state.setting.defaultOption,
        // limitMoveNum: 3,
        limitMoveNum: 30,
        singleHeight: 250,
        step:0,
      }

    };
  },
  computed: {
    sbtxSwiperFlag() {
      let ssyjSwiper = this.$store.state.setting.ssyjSwiper
      if (ssyjSwiper) {
        this.components = vueSeamlessScroll
      } else {
        this.components = Kong
      }
      return ssyjSwiper
    },
    sshFlag(){
      let ssh = this.$store.state.setting.sbtxSwiper
      return ssh
    }
  },
  created() {
    this.getData()
  },

  mounted() { },
  methods: {
    toUrl(item){
      if(item.ssh_port&&this.$store.state.setting.sbtxSwiper){
        //有映射端口，且setting配置允许才能打开url
        window.open('http://116.62.46.33:'+item.ssh_port+'/Pain/index.html', '_blank');
      }
     
    },
    clearData() {
      if (this.timer) {
        clearInterval(this.timer)
        this.timer = null
      }
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
    getData() {
      this.pageflag = true
      this.request.get("/hospital").then(res => {
        console.log("全国客户",res)
        this.list=res
        
      });
   
      let timer = setTimeout(() => {
        clearTimeout(timer)
        this.defaultOption.step=this.$store.state.setting.defaultOption.step
      }, this.$store.state.setting.defaultOption.waitTime);

      this.switper()

      // this.pageflag =false
      /*currentGET('big5', { limitNum: 50 }).then(res => {
        console.log('实时预警', res);
        if (res.success) {
          this.list = res.data.list
          let timer = setTimeout(() => {
              clearTimeout(timer)
              this.defaultOption.step=this.$store.state.setting.defaultOption.step
          }, this.$store.state.setting.defaultOption.waitTime);
        } else {
          this.pageflag = false
          this.$Message.warning(res.msg)
        }
      })*/
    },

  },
};
</script>
<style lang='scss' scoped>
.right_center {
  width: 100%;
  height: 100%;
  .pointer{
    cursor: pointer;
  }

  .right_center_item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: auto;
    padding: 10px;
    font-size: 14px;
    color: #fff;

    .orderNum {
      margin: 0 30px 0 -30px;
    }


    .inner_right {
      position: relative;
      height: 100%;
      width: 400px;
      flex-shrink: 0;
      line-height: 1.5;

      .dibu {
        position: absolute;
        height: 2px;
        width: 104%;
        background-image: url("../../assets/img/zuo_xuxian.png");
        bottom: -12px;
        left: -2%;
        background-size: cover;
      }
    }

    .info {
      //margin-right: 10px;
      //display: flex;
      //align-items: center;
      line-height: 1.5;
      overflow: hidden;

      .contents-right {
        width: 100%;
        display: flex;
        min-height: calc(100% - 60px);
        justify-content: space-between;
      }

      .labels {
        flex-shrink: 0;
        font-size: 16px;
        color: rgba(255, 255, 255, 0.6);
      }

      .zhuyao {
        color: $primary-color;
        font-size: 15px;
      }

      .ciyao {
        color: rgba(255, 255, 255, 0.8);
      }

      .warning {
        color: #E6A23C;
        font-size: 15px;
      }
    }

  }
}

.right_center_wrap {
  overflow: hidden;
  width: 100%;
  //height: 250px;
  height: 900px;
}

.overflow-y-auto {
  overflow-y: auto;
}
</style>
