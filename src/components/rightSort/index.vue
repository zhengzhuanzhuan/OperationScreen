<template>
<div>
  <div class= 'pic' v-if="sortData === 0">
       <img src="../../public/img/notData.png" alt="">
      <span>暂无数据</span>
    </div>
  <div class= 'screen_sort' v-else>
        <div class="logo-pic"> 
            <div class="logo">
                <img :class="{ 'logo-active': logoStatus === 0}" src="../../public/img/one.png" alt="">
                <!-- <p :class="{ 'p-active': logoStatus === 0}">NO.1</p> -->
                <p :class="{ 'p-active': logoStatus === 0}">{{ category_name[0] }}</p>
          </div>
          <div class="logo">
              <img  :class="{ 'logo-active': logoStatus === 1}" src="../../public/img/two.png" alt="">
              <!-- <p :class="{ 'p-active': logoStatus === 1}">NO.2</p> -->
              <p :class="{ 'p-active': logoStatus === 1}">{{ category_name[1] }}</p>
          </div>
          <div class="logo">
              <img :class="{ 'logo-active': logoStatus === 2}" src="../../public/img/three.png" alt="">
              <!-- <p :class="{ 'p-active': logoStatus === 2}">NO.3</p> -->
              <p :class="{ 'p-active': logoStatus === 2}">{{ category_name[2] }}</p>
          </div>
        </div>
       <div class="word">
          <span>销售额(元)</span>
          <span>销售量(件)</span>
      </div>
      <div class="count">
            <div class="total">
                <span class="test" v-show="logoStatus === 0" >{{sell_amount[0]}}</span>
                <span class="test" v-show="logoStatus === 1">{{sell_amount[1]}}</span>
                <span class="test" v-show="logoStatus === 2">{{sell_amount[2]}}</span>
            </div>
            <div class="total">
                <span class= 'test' v-show="logoStatus === 0">{{ sell_count[0] }}</span>
                <span class = 'test' v-show="logoStatus === 1">{{ sell_count[1] }}</span>
                <span class = 'test' v-show="logoStatus === 2">{{ sell_count[2] }}</span>
            </div>
      </div>    
   </div>
</div>
 
</template>
<style lang="scss" scoped>
.pic {
  text-align: center;
  margin-top: 30px;
  span {
    display: block;
    margin-top: 20px;
    text-align: center;
    opacity: 0.8;
    color: #fff;
    font-family: PingFangSC;
    font-size: 16px;
    letter-spacing: 0.8px;
    letter-spacing: 0.8px;
  }
img {
  width: 220px;
  vertical-align: top;
}
}
.logo-pic {
  height: 150px;
  margin-top: 10px;
  display: flex;
  justify-content: center;
  width: 100%;
  .logo {
    float: left;
    text-align: center;
    img {
    width: 80px;
    height: 80px;
    opacity: 0.5;
  }
    .logo-active {
    width: 100px;
    height: 100px;
    opacity: 1;
  }
  .p-active {
    opacity: 1;
  }
    p {
    color: #ffffff;
    margin: 0px 20px 0px;
    font-family: PingFangSC;
    font-size: 18px;
    font-weight: 600;
    opacity: 0.5;
  }
}
}
.screen_sort {
  text-align: center;
}
.word {
  padding-top: 15px;
  span {
    color: #ccc;
    padding: 10px 75px;
  }
  border-top: 1px solid #ccc;
}
.count {
  .total {
    float: left;
    width: 100px;
    height: 50px;
    // background-color: #fff;
    margin: 0px 66px;
  }
  span {
    font-size: 25px;
    text-align: center;
    color: white;
    display: inline-block;
    margin: 5px 0px 0 10px;
  }
}
.test {
  // 花费的时间和延时
  animation: test 0.5s 4.5s forwards;
}
@keyframes test {
  from {
    font-size: 25px;
    color: white;
  }
  to {
    font-size: 22px;
    color: rgba(255, 255, 255, 0.8);
  }
}
</style>
<script>
export default {
  // props: ['rightSort'],
  props: ['category_name', 'sell_amount', 'sell_count', 'hotSales', 'sortData'],
  data () {
    return {
      logoStatus: 0,
      logoActive: null,
      spanActive: null,
      bolean: null
    }
  },
  mounted () {
    clearInterval(this.logoActive)
    this.logoActive = setInterval(() => {
      if (this.logoStatus < 2) {
        this.logoStatus++
      } else {
        this.logoStatus = 0
      }
    }, 5000)
  }
}
</script>