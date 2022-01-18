<template>
    <div>
        6667777
        <button @click="ioopen"> inner </button>
        <button @click="$emit('close')"> close </button>
        <button @click="$emit('dispatcherCustomEvent','customEvent',true)"> customEvent </button>
        <dialog-view ref="dialog"></dialog-view>
    </div>
</template>
<script>
import DialogView from "./src/index"
export default{
    name:"fd",
    components:{
        DialogView
    },
    props:{
        test: [String,Number],
    },
    data(){
        return {

        }
    },
    mounted(){
        console.log(this.test)
    },
    methods:{
        ioopen(){
            this.$refs.dialog.open({
                width:"300px",
                title:"内部测试",
                component:null,
                isShowFullScreenBtn:false
            })
        },

    }
}
</script>

<style scoped>
div{
    height: 2000px !important;
    border:1px solid red;
}
</style>