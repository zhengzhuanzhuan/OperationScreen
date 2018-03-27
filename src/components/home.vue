<template>
  <div class="bg">
    <div id="screen" class="screen">
      <div class="header">
        <div class="header-title">
            <p>奇点云AI体验馆运营平台</p>
        </div>
        <div class="header-time">
          <p>{{ date }}</p>
          <p>{{ time }}</p>
        </div>
      </div>
      <div class="left">
           <div class="left-top">
             <div class="spanWord">
              <span>* 优惠劵核销暂为示例数据</span>
             </div>
                <left-coupon :coupon = 'total' ></left-coupon>
           </div>
           <div class="left-middle">
              <span>* 优惠劵核销暂为示例数据</span>
               <title-bar label="优惠订单价值占比"></title-bar>
                <div class="chart-wrap">
                    <left-scale :scale = 'total'></left-scale>
                </div>
           </div>
            <div class="left-bottom">
               <span>* 优惠劵核销暂为示例数据</span>
               <title-bar label="优惠劵状态"></title-bar>
                <div class="chart-wrap">
                    <left-state :dt_date = 'dt_date' :draw_coupon_count = 'draw_coupon_count' :used_coupon_count = 'used_coupon_count'></left-state>
                </div>
           </div>
      </div>
      <div class="middle">       
        <div class="middle-top" >
            <middle-swiper></middle-swiper>
        </div>
        <div class="middle-bottom">
           <title-bar label="销售转化"></title-bar>
                <div class="chart-wrap">
                    <middle-change :change= 'change'></middle-change>
                </div>
        </div>
      </div>
      <div class="right">
           <div class="right-top">
             <title-bar label="热销品类"></title-bar>
              <right-sort :sortData = 'sortData' :hotSales = 'hotSales' :category_name = 'category_name' :sell_amount = 'sell_amount' :sell_count = 'sell_count'></right-sort>
           </div>
           <div class="right-middle">
               <title-bar label="热销单品"></title-bar>
               <right-single :rightSingle = 'rightSingle' :first_sell = 'first_sell' :notNull = 'notNull'></right-single>
           </div>
            <div class="right-bottom">
               <title-bar label="店铺明细"></title-bar>
               <right-detail :swiperData = 'swiperData'></right-detail>
           </div>
      </div>
    </div>
  </div>
</template>

<script>
import setScreen from '@/utils/setScreen'
import common from '../utils/common.js'
import titleBar from './titleBar/index.vue'
import leftCoupon from './leftCoupon/index.vue'
import leftScale from './leftScale/index.vue'
import leftState from './leftState/index.vue'
import rightSort from './rightSort/index.vue'
import rightDetail from './rightDetail/index.vue'
import rightSingle from './rightSingle/index.vue'
import middleChange from './middleChange/index.vue'
import middleSwiper from './middleSwiper/index.vue'
import urls from './../utils/api.js'

export default {
  name: 'HelloWorld',
  data () {
    return {
      date: '',
      time: '',
      sortData: null,
      notNull: null,
      dateTimer: null,
      first_sell: [],
      draw_coupon_count: [],
      used_coupon_count: [],
      category_name: [],
      sell_amount: [],
      sell_count: [],
      dt_date: [],
      hotSales: '',
      axiosTimeOut: null,
      total: [
        {
          used_coupon_order_count: '0',
          coupon_used_rate: '0',
          draw_coupon: '0',
          with_coupon_rate: '0',
          without_coupon_rate: '0'
        }
      ],
      change: [
        {
          visitors_count: '',
          v_order_count: '',
          v_payed_count: '',
          v_repurchase_count: '',
          v_order_rate: '',
          v_payed_rate: '',
          v_repurchase_rate: ''
        }
      ],
      swiperData: [
        {
          title: [
            { name: '智能门店' },
            { name: '销售额(元)', value: '0.00' },
            { name: '销售件数', value: '0' },
            { name: '热销品类', value: '暂无' }
          ]
        },
        {
          title: [
            { name: '无人店' },
            { name: '销售额(元)', value: '0.00' },
            { name: '销售件数', value: '0' },
            { name: '热销品类', value: '暂无' }
          ]
        }
      ],
      rightSingle: []
    }
  },
  components: {
    titleBar,
    leftCoupon,
    leftScale,
    rightSort,
    rightDetail,
    rightSingle,
    leftState,
    middleChange,
    middleSwiper
  },
  methods: {
    getDate () {
      let _date = new Date()
      this.date = common.dateParse(_date, 'yyyy年MM月d日')
      this.time = common.dateParse(_date, 'hh:mm:ss')
    },
    timer () {
      this.getDate()
      clearInterval(this.dateTimer)
      this.dateTimer = setInterval(this.getDate, 1000)
    },

    // 优惠券状态，柱状图
    axiosCouponTrendDay () {
      this.$axios
        .get(urls.baseURL + urls.coupon_trend_day)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.draw_coupon_count = []
            this.used_coupon_count = []
            this.dt_date = []
            for (let i = 0; i < record.length; i++) {
              this.draw_coupon_count.push(record[i].draw_coupon_count)
              this.used_coupon_count.push(record[i].used_coupon_count)
              this.dt_date.push(record[i].dt_date.slice(4, -2) + '-' + record[i].dt_date.slice(-2))
              // console.log(this.dt_date)
            }
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 当日优惠券使用情况
    axiosCouponToday () {
      this.$axios
        .get(urls.baseURL + urls.coupon_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            // console.log(record)
            this.total[0].used_coupon_order_count =
              record.used_coupon_order_count
            this.total[0].coupon_used_rate =
              (record.coupon_used_rate * 100).toFixed(2) + '%'
            this.total[0].draw_coupon = record.draw_coupon_count
            this.total[0].with_coupon_rate =
              (record.with_coupon_rate * 100).toFixed(1) + '%'
            this.total[0].without_coupon_rate =
              (record.without_coupon_rate * 100).toFixed(1) + '%'
            // console.log(this.total[0].coupon_used_rate)
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 销售转化
    axiosSalesTransToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_trans_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.change[0].visitors_count = record.visitors_count
            this.change[0].v_order_count = record.v_order_count
            this.change[0].v_payed_count = record.v_payed_count
            this.change[0].v_repurchase_count = record.v_repurchase_count
            this.change[0].v_order_rate = record.v_order_rate
            this.change[0].v_payed_rate = record.v_payed_rate
            this.change[0].v_repurchase_rate = record.v_repurchase_rate
            // console.log(this.change[0].v_repurchase_rate, '===========')
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 店铺明细
    axiosSalesStoreToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_store_today)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.swiperData[1].title[1].value = (record[0].sell_amount).toFixed(2)
            this.swiperData[1].title[2].value = record[0].sell_count
            this.swiperData[0].title[1].value = (record[1].sell_amount).toFixed(2)
            this.swiperData[0].title[2].value = record[1].sell_count
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 店铺明细之热销品类
    axiosSalesCategory () {
      this.$axios
        .get(urls.baseURL + urls.sales_category)
        .then(res => {
          let data = res.data
          if (data.code === 0) {
            let record = data.data
            this.swiperData[1].title[3].value = record[0].category_name
            this.swiperData[0].title[3].value = record[1].category_name
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 热销单品
    axiosSalesGoodsToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_goods_today)
        .then(res => {
          let data = res.data
          // console.log(data, data.data.length)
          this.notNull = data.data.length
          if (data.code === 0) {
            let record = data.data
            // for (let i = 0; i < record.length; i++) {
            //   this.rightSingle[0].goods_id.push(record[i].goods_id)
            //   this.rightSingle[0].goods_name.push(record[i].goods_name)
            //   this.rightSingle[0].sell_amount.push(record[i].sell_amount)
            // }
            this.rightSingle = []
            for (let i = 0; i < record.length; i++) {
              this.rightSingle.push({
                rank_number: record[i].rank_number,
                goods_name: record[i].goods_name,
                sell_amount: record[i].sell_amount
              })
              this.first_sell.push(record[i].sell_amount)
            }
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 热销品类
    axiosSalesClassToday () {
      this.$axios
        .get(urls.baseURL + urls.sales_class_today)
        .then(res => {
          let hotSales = res.data
          if (hotSales.code === 0) {
            let record = hotSales.data
            this.sortData = record.length
            for (let i = 0; i < record.length; i++) {
              // this.rightSort.push({
              //   category_name: record[i].category_name,
              //   sell_amount: record[i].sell_amount,
              //   sell_count: record[i].sell_count
              // })
              this.category_name.push(record[i].category_name)
              this.sell_amount.push((record[i].sell_amount).toFixed(2))
              this.sell_count.push(record[i].sell_count)
            }
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    axiosAll () {
      this.axiosCouponTrendDay()
      this.axiosCouponToday()
      this.axiosSalesTransToday()
      this.axiosSalesStoreToday()
      this.axiosSalesCategory()
      this.axiosSalesGoodsToday()
      this.axiosSalesClassToday()
    }
  },
  mounted () {
    setScreen(1920, 1080, 'screen')
    this.timer()
    clearInterval(this.axiosTimeOut)
    this.axiosTimeOut = setInterval(() => {
      this.axiosAll()
    }, 3000)
  }
}
</script>

<style lang="scss" scoped>
.bg {
  width: 1920px;
  height: 1080px;
  overflow: hidden;
  .screen {
    overflow: hidden;
    width: 100%;
    height: 100%;
    padding: 20px 20px 0 20px;
    background-image: radial-gradient(circle at 47% 43%, #0e1f71, #0d093d);
    .header {
      margin-bottom: 30px;
      &-title {
        font-size: 54px;
        font-weight: 600;
        text-align: center;
        letter-spacing: 12px;
        font-family: PingFangSC;
        background-clip: text;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-image: linear-gradient(to left, #00bbff 0%, #00ffff);
      }
      &-time {
        text-align: right;
        position: absolute;
        right: 40px;
        top: 27px;
        color: #00bbff;
        font-family: DINAlternate;
        font-size: 26px;
        font-weight: bold;
      }
    }
    .left {
      float: left;
      width: 515px;
      span {
        float: right;
        color: #fff;
        opacity: 0.3;
        font-size: 14px;
      }
      & > div {
        height: 300px;
        margin-bottom: 20px;
        padding: 20px 24px;
        background-image: url('../public/img/small.png');
        // background-image: radial-gradient(circle at 47% 0, #6478a3, rgba(19, 54, 102, 0.1));
      }
      .left-top {
        padding: 0px;
      .spanWord {
        height: 10px;
        padding: 20px;
      }
      }
      .left-middle {
        .chart-wrap {
          width: 100%;
          height: 200px;
          margin-top: 30px;
        }
      }
    }
    .right {
      float: left;
      width: 515px;
      & > div {
        height: 300px;
        margin-bottom: 20px;
        padding: 20px 24px;
        background-image: url('../public/img/small.png');
        // background-image: radial-gradient(circle at 47% 0, #6478a3, rgba(19, 54, 102, 0.1));
      }
    }
    .chart-wrap {
      width: 100%;
      height: 250px;
      margin-top: 20px;
    }
    .middle {
      float: left;
      width: 810px;
      margin: 0 20px;
      &-top {
        height: 620px;
        margin-bottom: 20px;
        background-image: url('../public/img/middle_top.png');
      }
      &-bottom {
        height: 300px;
        padding: 20px 24px;
        background-image: url('../public/img/middle-bottom.png');
        .chart-wrap {
          width: 100%;
          height: 290px;
          line-height: 290px;
          // margin-top: 20px;
        }
      }
    }
  }
}
</style>
