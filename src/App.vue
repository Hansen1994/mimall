<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
import { computeAllGoodsSum } from './server/cartApi'
import { getUserInfo } from './server/UserApi'
export default {
  name: 'app',
  components: {
  },
  data () {
    return {
    }
  },
  mounted () {
    // 只有获得了用户信息才调用下面两个接口
    if (this.$cookie.get('userId')) {
      this.getUser()
      this.getCartCount()
    }
  },
  methods: {
    // post带true,get不带true
    async getUser () {
      const res = await getUserInfo()
      this.$store.dispatch('saveUserName', res.username)
      // 都给一个默认值以免未登入时，数据为空
      // this.axios.get('/user').then((res = {}) => {
      //   // 向Actions派发数据，参数一名字自定义，参数二为派发的数据
      //   this.$store.dispatch('saveUserName', res.username)
      // })
    },
    async getCartCount () {
      const res = await computeAllGoodsSum()
      this.$store.dispatch('saveCartCount', res)
      // this.axios.get('/carts/products/sum').then((res = 0) => {
      //   // todo vuex
      //   this.$store.dispatch('saveCartCount', res)
      // })
    }
  }
}
</script>

<style lang="scss">
  @import './assets/scss/reset.scss';
  @import './assets/scss/base.scss';
</style>
