<template>
  <div style="flex:1; ;margin-bottom: 50px;">

    <me-header-cell :message="data" @btnClick="showDialog"></me-header-cell>
    <me-wealth-cell :message="data" style="margin-top: 5px;"></me-wealth-cell>
    <divider-onepx></divider-onepx>
    <divider :message="{height:'10px'}"></divider>
    <divider-onepx></divider-onepx>
    <router-link :to="{path:'/coupons'}">
      <me-cell :message="{icon:require('../../assets/images/卡券.png'),title:'我的优惠券',warn:'即将过期',desc:'1张待使用'}"></me-cell>
    </router-link>
    <divider-onepx :message="{marginLeft:'10px'}"></divider-onepx>
    <me-cell :message="{icon:require('../../assets/images/钱包.png'),title:'一起赚',warn:'进行中',desc:'现金红包不停发'}"></me-cell>
    <divider-onepx></divider-onepx>
    <divider :message="{height:'10px'}"></divider>
    <me-grid-cell></me-grid-cell>

    <div v-transfer-dom>
      <x-dialog v-model="showScrollBox" class="dialog-demo" hide-on-blur>
        <me-dialog-score-bind-phone></me-dialog-score-bind-phone>
      </x-dialog>
    </div>
    <div v-transfer-dom>
      <x-dialog v-model="showScrollBox2" class="dialog-demo" hide-on-blur>
        <me-dialog-score-exchange ref="box" :message="data" @onScoreExchage="scoreExchage"
                                  @onCancle="cancleScoreExchangeDialog" @onExchange="exchangeScore"
        ></me-dialog-score-exchange>
      </x-dialog>
    </div>
    <div @click="showDialog2"
         style="width: 60px;height: 22px;background-color: white; position: fixed;top: 150px;right: 0px;
      display: flex;justify-content: center;align-items: center;
      border: solid 1px #2772ff;border-radius: 12px 0px 0px 12px">
      <img src="../../assets/images/user/gift.png" alt=" " style="align-self: center; width:15px; height:15px; ">
      <p style="color: #2772ff;font-size: 13px; margin-left: 3px;align-self: center;">兑换</p>
    </div>
  </div>

</template>

<script>
  import MeHeaderCell from './components/MeHeaderCell'
  import MeWealthCell from './components/MeWealthCell'
  import MeCell from './components/MeCell'
  import MeGridCell from './components/MeGridCell'
  import Divider from '../../components/Divider'
  import DividerOnepx from '../../components/Divider1px'

  import fetch from '../../api/http'
  import DividerVerticalOnepx from '../../components/DividerVertical1px'
  import MeDialogScoreExchange from './components/MeDialogScoreExchange.vue'
  import MeDialogScoreBindPhone from './components/MeDialogScoreBindPhone.vue'

  import {XDialog, XButton, Group, XSwitch, TransferDomDirective as TransferDom, XInput} from 'vux'

  export default {
    directives: {
      TransferDom
    },
    components: {
      MeHeaderCell,
      MeWealthCell,
      MeCell,
      Divider,
      MeGridCell,
      DividerOnepx,
      XDialog,
      XButton,
      Group,
      XSwitch,
      XInput,
      DividerVerticalOnepx,
      MeDialogScoreBindPhone,
      MeDialogScoreExchange
    },
    data() {
      return {
        showScrollBox: false,
        showScrollBox2: false,
        data: {
          headimgurl: '',
          mycoin: 0,
          mywallet: 0,
          nickname: "聪",
          withdrawable: 0,
          address: "北京怀柔",
          phone: "18857408796",
          postcode: "10001",
          realname: "whc",
        }
      }
    },
    methods: {
      showDialog() {
        this.showScrollBox = true
      },
      showDialog2() {
        this.showScrollBox2 = true
      },
      cancleScoreExchangeDialog() {
        this.showScrollBox2 = false;
      },
      scoreExchage() {
        this.showScrollBox2 = false;
        this.$router.push({path: '/scoreDeclare'});
      },
      exchangeScore(coins) {
        let url = 'http://tservice.prguanjia.com/account/exchange';
        fetch(url, {type: 'post', params: {coins: coins}}).then(res => {
          if (!res.message) {
            this.cancleScoreExchangeDialog()
            this.$vux.toast.show({
              text: '兑换成功', type: 'text'
            });
            this.fetchData()
          } else {
            this.$vux.toast.show({
              text: '兑换失败, 请稍后', type: 'text'
            });
          }
        }).catch(err => {
          this.$vux.toast.show({
            text: '兑换失败, 请稍后', type: 'text'
          });
        });

      },
      getAuthCode() {
        if (this.count < 60 && this.count > 0) {
          return;
        }

        if (!(/^1[34578]\d{9}$/.test(this.phone))) {
          this.$vux.toast.show({text: '手机号有误', type: 'text'});
          return;
        }
        fetch('http://tservice.prguanjia.com/account/sendAuthCode', {
          type: 'post',
          params: {phone: this.phone, state: 2}
        }).then(res => {
          this.startCountDown();
        }).catch(err => {

        });
      },
      startCountDown() {
        let self = this;
        var timer = null;
        timer = setInterval(function () {
          if (self.count > 0) {
            self.count--;
          }
          else {
            clearInterval(timer);
            self.count = 60;
          }
        }, 1000);
      },
      exchange() {
        this.showScrollBox2 = true;
      },
      fetchData() {
        fetch('http://tservice.prguanjia.com/account/home')
          .then(res => {
            this.data = res.data;
            console.log(this.data);
          }).catch(err => {
        })
      }
    },
    mounted() {
      this.fetchData()
    }
  }
</script>

<style lang="less" scoped>

  .dialog-demo {
    .weui-dialog {
      border-radius: 8px;
      padding-bottom: 8px;
    }
    .dialog-title {
      line-height: 30px;
      color: #666;
    }
    .img-box {
      height: 280px;
      width: 100% !important;
      overflow: hidden;
    }
  }
</style>
