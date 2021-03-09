<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"  @titleClick="titleClick"/>
    <scroll class="content" ref="scroll">
       <detail-swiper :top-images="topImages"/>
       <detail-base-info :goods="goods"/>
       <detail-shop-info :shop="shop"/>
       <detail-goods-info :detail-info="detailInfo" @load="imageLoad"/>
       <detail-param-info ref="params" :param-info="paramInfo" />
       <detail-comment-info ref="comment" :comment-info="commentInfo"/>
       <goods-list ref="recommend" :goods="recommends"/>
    </scroll>
    <detail-button-bar/>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailShopInfo from './childComps/DetailShopInfo'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo'
  import DetailParamInfo from './childComps/DetailParamInfo'
  import DetailCommentInfo from './childComps/DetailCommentInfo'
  import DetailButtonBar from './childComps/DetailButtonBar'


  import Scroll from 'components/common/scroll/Scroll'
  import GoodsList from 'components/content/goods/GoodsList'

  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from 'network/detail'

  export default {
    name: 'Detail',
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      DetailButtonBar,

      Scroll,
      GoodsList
    },
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: {},
        themeTopYs: []
      }
    },
    mounted () {
     
    },
    created () {
      // 1.保存传入的iid
      this.iid = this.$route.params.iid

      // 2.根据iid请求详情数据
      getDetail(this.iid).then(res => {
        // console.log(res);
        const data = res.result
        // 2.1获取顶部的图片轮播数据
        this.topImages = data.itemInfo.topImages

        // 2.2获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

        // 2.3获取店铺信息
        this.shop = new Shop(data.shopInfo)

        // 2.4获取商品详情
        this.detailInfo = data.detailInfo

        // 2.5获取商品参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

        // 2.6取出评论信息
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0]
        }

        this.$nextTick(() => {
          //根据最新的数据，对应的DOM是已经被渲染出来
          //但是图片依然是没有加载完(目前获取的offsetTop不包含其中的图片)
          //offsetTop值不对，一般都是图片的原因
          // this.themeTopYs = []

          // this.themeTopYs.push(0);
          // this.themeTopYs.push(this.$refs.params.$el.offsetTop);
          // this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
          // this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);

          console.log(this.themeTopYs);
        })
      })
      
      // 3.请求推荐数据
      getRecommend().then(res => {
        // console.log(res);
        this.recommends  = res.data.list
      })
    },
    methods: {
      imageLoad() {
        this.$ref.scroll.refresh()

        this.themeTopYs = []  
        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
      },
      titleClick(index) {
        console.log(index);
        this.$refs.scroll.scrollTo(0, this.themeTopYs[index], 200)
      }
    }
}
 
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px);
  }
</style>