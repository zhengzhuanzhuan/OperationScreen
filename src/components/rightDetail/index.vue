<template>
<swiper :options="swiperOption" ref="mySwiper">
    <swiper-slide  v-for='(item,index) in swiperData' :key="index">
        <div class="detail">
          <div class="logo">
              <a href="javascript:;"><img :src="img[index]" alt=""></a>
              <p>{{ item.title[0].name }}</p>
          </div>
          <div class="sales">
              <p>{{ item.title[1].name }}</p>
              <span>{{ item.title[1].value }}</span>
          </div>
          <div class="count">
            <p>{{ item.title[2].name }}</p>
            <span>{{ item.title[2].value }}</span>
          </div>
          <div class="kind">
            <p>{{ item.title[3].name }}</p>
            <span>{{ item.title[3].value }}</span>
          </div>
        </div>
    </swiper-slide>
  </swiper>  
 
</template>
<style lang="scss" scoped>
.detail {
  position: relative;
  .logo {
    margin: 30px;
    a {
      display: inline-block;
      width: 100px;
      height: 100px;
      background-color: #ffffff;
      border-style: solid;
      border-width: 2px;
      border-image-source: linear-gradient(to right, #ff007c, #fa9696);
      border-image-slice: 1;
    }
    p {
      color: #ffffff;
      margin: 15px 20px;
      font-family: PingFangSC;
      font-size: 18px;
      font-weight: 600;
    }
  }
  .sales {
    position: absolute;
    right: 174px;
    top: 4px;
    span {
      font-family: DINAlternate;
      font-size: 50px;
      // font-weight: bold;
      color: #ffffff;
    }
    p {
      // opacity: 0.6;
      font-size: 18px;
    }
  }
  .count {
    position: absolute;
    right: 205px;
    top: 120px;
    span {
      font-family: DINAlternate;
      font-size: 36px;
      // font-weight: bold;
      color: #ffffff;
    }
  }
  .kind {
    position: absolute;
    right: 50px;
    top: 125px;
    text-align: center;
    p {
    padding-bottom: 7px;
    }
    span {
      font-family: PingFangSC;
      font-size: 20px;
      // font-weight: 600;
      text-align: left;
      color: #ffffff;
    }
  }
  p {
    // opacity: 0.6;
    font-family: PingFangSC;
    font-size: 14px;
    letter-spacing: 0.8px;
    margin-bottom: 5px;
    color: #cccccc;
  }
}
.swiper-container {
  width: 100%;
  height: 100%;
}
</style>
<script>
import nobody from '../../public/img/nobody.png'
import brain from '../../public/img/brain.png'
export default {
  name: 'hello-world',
  props: ['swiperData'],
  data () {
    return {
      swiperOption: {
        autoplay: {
          delay: 10000,
          stopOnLastSlide: false,
          disableOnInteraction: false
        },
        pagination: '.swiper-pagination',
        direction: 'horizontal',
        slidesPerView: 1
      },
      img: [brain, nobody]
    }
  },
  methods: {
    axiosAll () {
    }
  },
  mounted () {
  }
}
</script>