<template>
  <div class="product">
    <product-param :title="product.name">
      <template v-slot:buy>
        <button class="btn" v-on:click="buy">立即购买</button>
      </template>
    </product-param>
    <div class="content">
      <div class="item-bg">
        <h2>{{product.name}}</h2>
        <h3>{{product.subtitle}}</h3>
        <p>
          <a href="" id="">全球首款双频 GP</a><span>|</span>
          <a href="" id="">骁龙845</a><span>|</span>
          <a href="" id="">AI 变焦双摄</a><span>|</span>
          <a href="" id="">红外人脸识别</a>
        </p>
        <div class="price">
          <span>￥<em>{{product.price}}</em></span>
        </div>
      </div>
      <div class="item-bg-2"></div>
      <div class="item-bg-3"></div>
      <div class="item-swiper">
        <swiper :options="swiperOption">
          <swiper-slide><img src="/imgs/product/gallery-2.png" alt=""></swiper-slide>
          <swiper-slide><img src="/imgs/product/gallery-3.png" alt=""></swiper-slide>
          <swiper-slide><img src="/imgs/product/gallery-4.png" alt=""></swiper-slide>
          <swiper-slide><img src="/imgs/product/gallery-5.jpg" alt=""></swiper-slide>
          <swiper-slide><img src="/imgs/product/gallery-6.jpg" alt=""></swiper-slide>
          <!-- Optional controls -->
          <div class="swiper-pagination"  slot="pagination"></div>
        </swiper>
        <p class="desc">小米8 AI变焦双摄拍摄</p>
      </div>
      <div class="item-video">
        <h2>60帧超慢动作摄影<br/>慢慢回味每一瞬间的精彩</h2>
        <p>后置960帧电影般超慢动作视频，将眨眼间的美妙展现得淋漓尽致！<br/>更能AI 精准分析视频内容，15个场景智能匹配背景音效。</p>
        <div class="video-bg" @click="showVideo='slideDown'"></div>
        <!-- v-show == ''相当于flase相当于display=none -->
        <!-- 这里相当于动画时间结束后为''就行 ，但是不能手动去设置true和false-->
        <div class="video-box" v-show="showVideo">
          <div class="overlay"></div>
          <div class="video" :class="showVideo">
            <span class="icon-close" @click="closeVideo"></span>
            <video src="/imgs/product/video.mp4" muted autoplay controls="controls"></video>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import ProductParam from './../components/ProductParam'
import { getProductionDetail } from './../server/ProductionApi'
export default {
  name: 'product',
  components: {
    ProductParam,
    swiperSlide,
    swiper
  },
  data () {
    return {
      // 控制动画
      showVideo: '',
      // 定义产品
      product: '',
      swiperOption: {
        autoplay: true,
        // 一版几个
        slidesPerView: 3,
        spaceBetween: 30,
        freeMode: true,
        pafination: {
          el: '.swiper-pagination',
          clickable: true
        }
      }
    }
  },
  mounted () {
    this.getProductInfo()
  },
  methods: {
    async getProductInfo () {
      // 获取路由id
      const id = this.$route.params.id
      const res = await getProductionDetail(id)
      this.product = res
      // this.axios.get(`/products/${id}`).then((res) => {
      //   // console.log(res)
      //   this.product = res
      // })
    },
    buy () {
      const id = this.$route.params.id
      this.$router.push(`/detail/${id}`)
    },
    closeVideo () {
      this.showVideo = 'slideUp'
      setTimeout(() => {
        this.showVideo = ''
      }, 600)
    }
  }
}

</script>

<style lang="scss">
@import './../assets/scss/mixin.scss';
.product{
  button{
    margin-left: 15px;
  }
  .content{
    .item-bg{
      background:url('/imgs/product/product-bg-1.png') no-repeat center;
      height:718px;
      text-align:center;
      h2{
        font-size: 80px;
        padding-top: 55px;
      }
      h3{
        font-size: 24px;
        letter-spacing: 10px;
      }
      p{
        margin-top: 21px;
        margin-bottom: 40px;
        a{
          font-size: 16px;
          color: #333;
        }
        span{
          margin: 0 15px;
        }
      }
      .price{
        font-size: 30px;
        color: #333;
        em{
          font-style: normal;
          font-size: 38px;
        }
      }
    }
    .item-bg-2{
      background:url(/imgs/product/product-bg-2.png) no-repeat center;
      height:480px;
      background-size:1226px 397px;
    }
    .item-bg-3{
      background:url(/imgs/product/product-bg-3.png) no-repeat center;
      height:638px;
      background-size:cover;
    }
    .item-swiper{
      margin:36px auto 52px;
      .desc{
        font-size:18px;
        color:#333333;
        text-align:center;
      }
      img{
        width:100%;
      }
    }
    .item-video{
      height: 1044px;
      background-color: #070708;
      margin-bottom: 76px;
      color: #fff;
      text-align: center;
      h2{
        font-size: 60px;
        padding-top: 82px;
        margin-bottom: 47px;
      }
      p{
        font-size: 24px;
        margin-bottom: 58px;
      }
      .video-bg{
        background:url('/imgs/product/gallery-1.png') no-repeat center;
        background-size: 900px 500px;
        width: 1226px;
        height: 540px;
        margin: 0 auto 120px;
        cursor: pointer;
      }
      .video-box{
        .overlay{
          @include position(fixed);
          background-color: #333;
          opacity: .4;
          z-index: 10;
        }
        // 这些动画只有先定义好了才能用
        @keyframes slideDown {
          0%{
            top: -50%;
            opacity: 0;
          }
          50%{
            top: 25%;
            left: 75%;
            opacity: 0.5;
          }
          100%{
            top: 50%;
            left: 50%;
            opacity: 1;
          }
        }
        @keyframes slideUp {
          0%{
            top: 50%;
            left: 50%;
            opacity: 1;
          }
          50%{
            top: 25%;
            left: 25%;
            opacity: 0.5;
          }
          100%{
            top: -50%;
            left: 50%;
            opacity: 0;
          }
        }
        .video{
          position: fixed;
          top: -50%;
          left: 50%;
          transform: translate(-50%, -50%);
          z-index: 10;
          width: 1000px;
          height: 536px;
          opacity: 1;
          // 使用自定义动画
          &.slideDown{
            // 过度时间和速度
            animation: slideDown .6s linear;
            // 为了让动画移进来时的最后一帧固定
            top: 50%;
          }
          &.slideUp {
            animation: slideUp .6s linear;
          }
          .icon-close{
            @include positionImg(absolute, 20px, 20px, null, null, 20px, 20px, '/imgs/icon-close.png');
            z-index: 11;
          }
          video{
            width: 100%;
            height: 100%;
            // 填满视频周边的阴影
            object-fit: cover;
            // 出去周边边框
            outline: none;
          }
        }
      }
    }
  }
}
</style>
