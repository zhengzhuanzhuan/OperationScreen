<template>
 <div>
    <div class="total">
       <p>今日使用优惠订单数(笔)</p>
       <span>{{ coupon[0].used_coupon_order_count== null? 0 : coupon[0].used_coupon_order_count  }}</span>
    </div>
      <div class="use">
          <div class="word">
                <p>已发放</p>
                <span>{{ coupon[0].draw_coupon }}</span>
            </div>
            <div class="count">
              <p>核销率</p>
                <span>{{ coupon[0].coupon_used_rate }}</span>
            </div>
      </div>
 </div>
</template>
<style lang="scss" scoped>
div {
  color: #fff;
  position: relative;
  .total {
    text-align: center;
    p {
      font-family: PingFangSC;
      font-size: 20px;
      font-weight: 600;
      padding: 20px 0 6px;
      letter-spacing: 1.4px;
    }
    span {
      color: #00ffff;
      font-family: DINAlternate;
      font-size: 60px;
      font-weight: bold;
    }
  }
  .use {
    height: 120px;
    width: 100%;
    position: absolute;
    top: 140px;
    background-color: #2A3675;
   display: flex;
  justify-content: center;
    align-items: center;
  text-align: center;
  .word {
    margin-right: 80px;
  }
  .count {
      margin-left: 80px;
  }
    p {
         opacity: 0.8;
        font-family: PingFangSC;
        font-size: 18px;
        letter-spacing: 1.3px;
        margin-bottom: 5px;
    }
    // span {
    //   display: inline-block;
    //   padding: 23px 80px 15px 90px;
    // }
    span {
        font-family: DINAlternate;
        font-size: 30px;
        font-weight: bold;
        letter-spacing: 1.6px;
    }
}
}
</style>
<script>

export default {
  props: [ 'coupon' ],
  data () {
    return {
    }
  },
  methods: {
  },
  mounted () {
  }
}
</script>