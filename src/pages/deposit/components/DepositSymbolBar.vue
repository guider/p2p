<template>
  <div style="display: flex;flex-direction: row; height: 44px;width: 100%; align-items: center;">
    <p style="display:flex;flex: 0.5; margin-left: 10px; align-items: center;justify-content: center; font-size: 15px;color: #333">
      {{indexTitle(0)}}
    </p>
    <p class="wrapItem" style="margin-left: 0;">
      {{indexTitle(1)}}
    </p>
    <p class="wrapItem" style="margin-left: 0; ">
      {{indexTitle(2)}}
    </p>
    <p class="wrapItem" style="margin-left: 0;">
      {{indexTitle(3)}}
    </p>
  </div>

</template>
<script>
  export default {
    name: 'symbol',
    props: ['message'],
    mounted() {

    },
    methods: {
      indexTitle(i) {
        return this.message[i];
      },
      titleLength() {
        return this.message.length() / 5;
      }
    },
    computed: {}
  }
</script>
<style scoped>

  .wrapItem {
    margin-left: 4px;
    margin-right: 4px;
    font-size: 15px;
    color: #333;
    text-align: center;
    flex:1;
  }


</style>
