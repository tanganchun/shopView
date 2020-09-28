<template>
    <div id="home">
        <!--        头部导航-->
        <nav-bar class="home-nav">
            <div slot="center">购物街</div>
        </nav-bar>

        <!--        选项卡-->
        <tab-control :titles="['流行','新款','精选']"
                     ref="tabControl1"
                     @tabClick="tabClick"
                     class="tab-control"
                     v-show="isTabFixed"/>

        <scroll class="content"
                ref="scroll"
                :probe-type="3"
                :pull-up-load="true"
                @scroll="contentScroll"
                @pullingUp="loadMore"
        >

            <!--        轮播图-->
            <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>

            <!--        选秀兰-->
            <recommend-view :recommends="recommends"/>

            <!--        连接大图-->
            <feature-view/>

            <!--        选项卡-->
            <tab-control :titles="['流行','新款','精选']"
                         ref="tabControl2"
                         @tabClick="tabClick"
                         />

            <!--        商品组件-->
            <goods-list :goods="showGoods"/>

        </scroll>

        <!--        回到顶部按钮-->
        <back-top @click.native="backClick" v-show="flag"/>
    </div>
</template>

<script>
    // 公共组件
    import NavBar from "components/common/navbar/NavBar";
    import TabControl from "components/content/tabControl/TabControl";
    import GoodsList from "components/content/goods/GoodsList";
    import Scroll from "components/common/scroll/Scroll";
    import BackTop from "components/content/backTop/BackTop";

    // 子组件
    import HomeSwiper from "./childComps/HomeSwiper";
    import RecommendView from "./childComps/RecommendView";
    import FeatureView from "./childComps/FeatureView";

    // 请求
    import {
        getHomeMultidata,
        getHomeGoods
    } from "network/home";

    // 使用的方法
    import {debounce} from 'common/utils'

    export default {
        name: "home",
        data() {
            return {
                banners: [],
                recommends: [],
                goods: {
                    'pop': {page: 0, list: []},
                    'new': {page: 0, list: []},
                    'sell': {page: 0, list: []}
                },
                currentType: 'pop',
                flag: false,
                tabOffsetTop: 0,
                isTabFixed: false,
                saveY : 0
            }
        },
        components: {
            // 公共组件
            NavBar,
            TabControl,
            GoodsList,
            Scroll,
            BackTop,

            // 子组件
            HomeSwiper,
            RecommendView,
            FeatureView
        },
        created() {
            //1. 请求多个数据 请求轮播图的数据
            this.getHomeMultidata()

            // 请求商品数据
            this.getHomeGoods('pop', 1)
            this.getHomeGoods('new', 1)
            this.getHomeGoods('sell', 1)
        },
        mounted() {
            // 监听图片加载itemImageLoad
            const refresh = debounce(this.$refs.scroll.refresh, 150)
            this.$bus.$on('itemImageLoad', () => {
                refresh()
            })
        },
        methods: {

            // 轮播图数据
            getHomeMultidata() {
                getHomeMultidata().then(value => {
                    // 进行保存
                    this.banners = value.data.banner.list
                    this.recommends = value.data.recommend.list
                })
            },

            // 商品数据
            getHomeGoods(type) {
                const page = this.goods[type].page + 1

                getHomeGoods(type, page).then(value => {
                    this.goods[type].list.push(...value.data.list)
                    this.goods[type].page += 1
                    // 清除下拉事件scroll的 更新
                    this.$refs.scroll.finishPullUp()
                })
            },

            // 监听选项卡
            tabClick(index) {
                switch (index) {
                    case 0:
                        this.currentType = 'pop'
                        break
                    case 1:
                        this.currentType = 'new'
                        break
                    case 2:
                        this.currentType = 'sell'
                }
                this.$refs.tabControl1.currentIndex = index
                this.$refs.tabControl2.currentIndex = index
            },
            // 一件回顶部
            backClick() {
                this.$refs.scroll.scrollTo(0, 0)
            },
            // 监听滚动位置
            contentScroll(position) {
                // 判断 backTop 是否显示
                this.flag = (-position.y) > 1000

                // 决定 tabControl 是否固定
                this.isTabFixed = (-position.y) > this.tabOffsetTop
            },
            // 监听滚动到底部 再次去拿数据
            loadMore() {
                this.getHomeGoods(this.currentType)

            },
            // 等待轮播图加载完毕 获取选项栏距离顶部的高度
            swiperImageLoad() {
                this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
            }
        },
        computed: {
            showGoods() {
                return this.goods[this.currentType].list
            }
        },
        // 将页剧保留
        activated() {
           this.$refs.scroll.scrollTo(0,this.saveY,0)
            this.$refs.scroll.refresh()
        },
        // 保存数据
        deactivated() {
           this.saveY = this.$refs.scroll.getScrollY()
        }
    }
</script>

<style scoped>
    #home {
        padding-top: 44px;
        height: 100vh;
        position: relative;
    }

    .home-nav {
        background-color: red;
        color: white;
        margin-top: -44px;
    }


    .content {
        position: absolute;
        overflow: hidden;
        top: 44px;
        bottom: 49px;
        left: 0;
        right: 0;
    }

    .tab-control{
        position: relative;
        z-index: 3;
    }
</style>
