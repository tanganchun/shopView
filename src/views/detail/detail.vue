<template>
    <div id="detail">
        <!--        顶部导航跳-->
        <detail-nav-bar/>

        <!--        轮播图-->
        <detail-swiper :top-images="topImages"/>
    </div>
</template>

<script>
    // 公共组件

    // 子组件
    import DetailNavBar from "./childComponents/DetailNavBar";
    import DetailSwiper from "./childComponents/DetailSwiper";

    // 请求数据
    import {getDetail} from "network/detail";

    export default {
        name: "detail",
        data() {
            return {
                iid: null,
                topImages: [],
                goods:{}
            }
        },
        created() {
            // 保存传入的 iid
            this.iid = this.$route.params.iid

            // 根据iid 请求详情数据.getDetail
            getDetail(this.iid).then(res => {
                // 获取顶部的图片轮播数据
                this.topImages = res.result.itemInfo.topImages
                console.log(res)
                // 请求商品数据
                this.goods = {
                    title : res.itemInfo,

                }
            })
        },
        components: {
            DetailNavBar,
            DetailSwiper
        }
    }
</script>

<style scoped>

</style>