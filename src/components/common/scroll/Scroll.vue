<template>
    <div class="wrapper" ref="wrapper">
        <div>
            <slot></slot>
        </div>
    </div>
</template>


<script>
    import betterScroll from 'better-scroll'

    export default {
        name: "Scroll",
        data(){
          return {
              scroll : null
          }
        },
        props:{
            probeType:{
                type :Number,
                default:0
            },
            pullUpLoad:{
                type:Boolean,
                default: false
            }
        },
        mounted() {
            this.scroll = new betterScroll(this.$refs.wrapper,{
                movable: true,
                zoom: true,
                click:true,
                probeType:this.probeType,
                pullUpLoad: this.pullUpLoad
            })
            // 监听滚动的位置
            if(this.probeType === 2 || this.probeType === 3){
                this.scroll.on('scroll',position => {
                    this.$emit('scroll',position)
                })
            }

            // 监听上拉事件
            if(this.pullUpLoad){
                this.scroll.on('pullingUp',()=>{
                    this.$emit('pullingUp')
                })
            }
        },
        methods:{
            scrollTo(x,y,time=500){
                this.scroll && this.scroll.scrollTo(x,y,time)
            },
            finishPullUp(){
                this.scroll && this.scroll.finishPullUp()
            },
            refresh(){
                this.scroll && this.scroll.refresh()
            },
            getScrollY(){
                return this.scrollTo ? this.scroll.y : 0
            }
        }
    }
</script>

<style scoped>


</style>