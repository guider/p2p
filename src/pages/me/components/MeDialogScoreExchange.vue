<template>
  <div class="img-box" style="width: 100%;display: flex;flex-direction: column;height: 320px;" ref="box">
    <p @click="toScoreExchage"
       style="display: flex;color: #999999; font-size: 12px;align-items: center;margin-top: 10px;align-self: flex-end;margin-bottom: 25px;margin-right: 20px;">
      <img src="../../../assets/images/sign/问号.png"
           style="width: 10px;height: 10px;padding:1px;border: solid 1px #bfbfbf;border-radius: 7px;"
           alt=" ">
      &nbsp;&nbsp;积分规则</p>

    <div style="display: flex;margin-left: 30px;margin-right: 30px;">

      <button class="calc" @click="minus" :disabled="minusStatus">一</button>
      <div style=" display:flex;flex: 1;justify-content: center;align-self: center;">
        <input type="number" v-model="score">
        <span class="score">积分</span>
      </div>
      <button class="calc" style="font-size: 18px;" @click="add" :disabled="addStatus">＋</button>

    </div>

    <p style="color: #999999; font-size: 10px; text-align: left; margin: 20px 20px 15px 20px;">
      提示：提现金额以元为单位，须为10的整数倍。</p>
    <div
      style="flex: 1;width: 100%;justify-content: center;align-items: center;align-self: center;flex-direction: column;">
      <div id="arrow"
           style="position:relative;left: 0 ;right: 0;margin: auto;top: 5.6px;background-color: white;"></div>
      <divider-onepx style="flex: 1px;"></divider-onepx>
      <div
        style="display: flex;flex-direction: column;justify-content: center;align-self: center;
              width: 100%;height:100%;border-radius: 0 0 2px 2px;background-color: #eee;">

        <p style="font-size:34px;color: #ff6600;margin-top: 20px;">{{parseInt(score / 100)}}.00<span
          style="color:#333;font-size: 12px;">元</span>
        </p>

        <div
          style="width: 100%;display: flex; height: 32px;flex-direction: row;margin-top: 18px;margin-bottom: 45px;">
          <div style="display: flex;flex: 1;">
            <x-button mini type="primary" style="width: 120px;color: white" @click.native="doExchange">确定</x-button>
          </div>
          <div style="flex: 1;display: flex;">
            <x-button mini style="width: 120px;background-color: #999;color: white;" @click.native="doCancle">取消
            </x-button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>


  import {XDialog, XButton, Group} from 'vux'
  import DividerOnepx from '../../../components/Divider1px'

  export default {
    props: ['message'],
    components: {
      XDialog,
      XButton,
      Group,
      DividerOnepx
    },
    data() {
      return {
        score: 100

      }
    },
    methods: {
      toScoreExchage() {
        this.$emit("onScoreExchage")
      },
      doExchange() {
        if (this.message.mycoin < this.score) {
          this.$vux.toast.show({
            text: '超过可用金币数量', type: 'text'
          });
          return;
        }
        if (this.score < 100) {
          this.$vux.toast.show({
            text: '至少兑换100个', type: 'text'
          });
          return;
        }
        if (this.score % 100) {
          this.$vux.toast.show({
            text: '请输入100的整数倍', type: 'text'
          });
          return;
        }

        this.$emit("onExchange", this.score)
      },
      doCancle() {
        this.$emit("onCancle")
      },
      minus() {
        if (this.score > 100) {
          this.score -= 100;
        }
      },
      add() {
        this.score += 100;

        if (this.score < this.message.mycoin - 100) {
          this.score += 100;
        }
      }
    },
    computed: {
      minusStatus() {
        return this.score <= 100
      },
      addStatus() {
        return this.score >= this.message.mycoin - 100
      }

    },
    mounted() {
      this.$refs.box.parentNode.style.maxWidth = '92%';
      this.$refs.box.parentNode.style.width = '92%';
    }
  }
</script>

<style scoped>

  #arrow {
    display: block;
    height: 8px;
    width: 8px;

    border-width: 1px;
    border-color: transparent;
    border-style: solid;
    border-bottom-color: #d8d8d8;
    border-left-color: #d8d8d8;
    transform: rotate(-45deg);
  }

  input {
    color: #2772ff;
    font-size: 18px;
    border-radius: 4px;
    border: solid 1px #bfbfbf;
    width: 80px;
    height: 30px;
    text-shadow: none;
    box-shadow: none;
    outline: none;
    padding-right: 5px;
    padding-left: 5px;
  }

  .calc {
    border: solid 1px #666666;
    background-color: white;
    border-radius: 18px;
    align-self: center;
    justify-content: center;
    outline: none;
    line-height: 36px;
    width: 36px;
    height: 36px;
    align-items: center;
    text-align: center;
    font-size: 15px;
    color: #666666;
  }

  .calc:disabled {
    border: solid 1px #eee;
    background-color: white;
    border-radius: 18px;
    align-self: center;
    justify-content: center;
    align-items: center;
    text-align: center;
    outline: none;
    line-height: 36px;
    width: 36px;
    height: 36px;
    font-size: 15px;
    color: #ccc;
  }

  .score {
    color: #666666;
    font-size: 14px;
    text-align: center;
    display: flex;
    align-self: center;
    margin-left: 8px;
  }
</style>
