<template>
  <wechat-emotion-input @submit="submit"/>
</template>

<script>
  import wechatEmotionInput from "../components/WechatEmotionInput"
  export default {
    components: {
      wechatEmotionInput
    },
    data() {
      return {}
    },
    created() {
      this.getData()
    },
    methods: {
      getData() {

      },
      submit(html){
        console.log(html)
      }
    }
  }
</script>

<style lang="less" scoped>

</style>
