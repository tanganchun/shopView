<template>
    <tab-bar>
        <tar-bar-item path="/home" activeColor="deepPink">
            <img slot="item-icon" src="~assets/img/tabbar/home.svg" alt="">
            <img slot="item-icon-active" src="~assets/img/tabbar/home_active.svg" alt="">
            <div slot="item-text">首页</div>
        </tar-bar-item>
        <tar-bar-item path="/category" activeColor="deepPink">
            <img slot="item-icon" src="~assets/img/tabbar/category.svg" alt="">
            <img slot="item-icon-active" src="~assets/img/tabbar/category_active.svg" alt="">
            <div slot="item-text">分类</div>
        </tar-bar-item>
        <tar-bar-item path="/cart" activeColor="deepPink">
            <img slot="item-icon" src="~assets/img/tabbar/cart.svg" alt="">
            <img slot="item-icon-active" src="~assets/img/tabbar/cart_active.svg" alt="">
            <div slot="item-text">购物车</div>
        </tar-bar-item>
        <tar-bar-item path="/profile" activeColor="deepPink">
            <img slot="item-icon" src="~assets/img/tabbar/profile.svg" alt="">
            <img slot="item-icon-active" src="~assets/img/tabbar/profile_active.svg" alt="">
            <div slot="item-text">我的</div>
        </tar-bar-item>
    </tab-bar>
</template>

<script>
    import tabBar from "../../common/tabBar/tabBar";
    import tarBarItem from "../../common/tabBar/tarBarItem";

    export default {
        name: "MainTabBar",
        components: {
            tabBar,
            tarBarItem
        }
    }
</script>

<style scoped>

</style>
