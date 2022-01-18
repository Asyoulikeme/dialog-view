<template>
    <div>
        <button @click="open1">click me open1</button>
        <button @click="open2">click me open2</button>
        <dialog-view ref="dialog" @close="onClose" @customEvent="customEvent">
            <div slot="footer">footer-666</div>
        </dialog-view>
    </div>
</template>

<script>
import DialogView from "./src/index"
import test from "./test/test"
export default {
    name: "demo",
    components:{
        DialogView
    },
    data() {
        return {
        
        }
    },
    async mounted(){
        //this.open()
    },
    methods: {
        open1(){
            this.$refs.dialog.open({
                title:"查看详情<i>hash</i>",
                top:"10px",
                component:test,
                width:"375px",
                componentProps:{
                    test:1234567890
                },
                //isFullScreen:true,
                injectData:{

                },
            })
        },
        onClose(){console.log("调用一次")},
        customEvent(value){
            console.log("自定义事件中心成功调用")
            console.log(value)
        },
        open2(){
            this.$refs.dialog.open({
                title:"查看详情<i>hash</i>",
                component:{
                    render(h){
                        return h('div',{},"123")
                    }
                },
                isShowFullScreenBtn:false,
                height:"auto",
                injectData:{

                }
            })
        }
    
    }
}
</script>
