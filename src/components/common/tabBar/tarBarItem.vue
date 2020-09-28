<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>

    <div class="item-active-icon" v-else>
      <slot name="item-icon-active"></slot>
    </div>

    <div :style="activeStyle" class="item-text">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    name: "tarBarItem",
    data() {
      return {

      }
    },
    props:{
      path : String,
      activeColor:{
        type : String,
        default : 'red'
      }
    },
    computed:{
      isActive(){
        return this.$route.path.indexOf(this.path) !== -1
      },
      activeStyle(){
        return this.isActive ? {color : this.activeColor} : {}
      }
    },
    methods:{
      itemClick(){
        this.$router.push(this.path)
      }
    }
  }
</script>

<style scoped>
  .tab-bar-item {
    flex: 1;
    text-align: center;
    height: 49px;
    font-size: 14px;
  }

  .tab-bar-item img {
    width: 24px;
    height: 24px;
    margin-top: 5px;
    vertical-align: middle;
  }
  .item-text {
    font-size: 12px;
    margin-top: 3px;
    color: #333;
  }

</style>
