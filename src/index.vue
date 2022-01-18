<script>
import { useMiniRouter} from "mini-router"
export default {
  name: "DialogView",
  data() {
    return {
      dialog: {
        title: "",
        name: "",
        top:"10vh",
        visible: false,
        width: "80%",
        height:"calc(100vh - 20vh)",
        component: null,
        injectData: undefined,
        isFullScreen: false,
        isShowFullScreenBtn: true
      },
      dialogClassName: "",
      cachedHeight:"calc(100vh - 20vh)",
      componentProps:{},
      /* 赋予弹窗组件更强的能力 --组件缓存 */
      unCachedView: [],
      /* 赋予弹窗组件更强的能力 --动态组件跳转传参 */
      miniRouteParams: {},
    };
  },
  computed:{
    svgIcon(){
      return !this.dialog.isFullScreen ?
      `<svg t="1608189548420" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="12033" width="16" height="16"><path d="M475.3408 850.5344h-266.24a37.2736 37.2736 0 0 1-37.2736-37.2736v-266.24a37.2736 37.2736 0 1 1 74.5472 0v228.9664h228.9664a37.2736 37.2736 0 0 1 0 74.5472zM830.3104 495.5648a37.2736 37.2736 0 0 1-37.2736-37.2736V229.3248h-228.9664a37.2736 37.2736 0 1 1 0-74.5472h266.24a37.2736 37.2736 0 0 1 37.2736 37.2736v266.24a37.2736 37.2736 0 0 1-37.2736 37.2736z" p-id="12034" fill="#909399"></path></svg>`
      :
      `<svg t="1610971062197" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4341" width="16" height="16"><path d="M411.741091 558.545455a34.909091 34.909091 0 0 1 34.909091 34.90909v272.104728a34.909091 34.909091 0 1 1-69.818182 0v-187.810909L106.146909 948.433455a34.792727 34.792727 0 0 1-24.669091 10.216727 34.909091 34.909091 0 0 1-24.692363-59.578182L327.493818 628.363636H139.636364a34.909091 34.909091 0 1 1 0-69.818181h272.104727z m487.330909-501.76a34.909091 34.909091 0 1 1 49.338182 49.338181L677.748364 376.832h187.857454a34.909091 34.909091 0 1 1 0 69.818182H593.454545a34.909091 34.909091 0 0 1-34.90909-34.909091V139.636364a34.909091 34.909091 0 1 1 69.818181 0v187.810909L899.072 56.785455z" fill="#797979" p-id="4342"></path></svg>`
    }
  },
  mounted() {
    this.setClassName();
  },
  methods: {
    /**
     * @param component { Object } 需要渲染的组件
     * @param title { string } dialog标题
     * @param injectData { any } dialog子组件需要使用的数据
     * @param width { string } dialog宽度
     * @param isFullScreen
     * @param isShowFullScreenBtn { boolean } dialog宽度
     */
    open({
      component = null,
      title = "",
      name = "",
      injectData = undefined,
      width = "80%",
      height = "calc(100vh - 20vh)",
      isFullScreen = false,
      isShowFullScreenBtn = true,
      top = "10vh",
      componentProps = {}
    }) {
      this.dialog.component = component;
      this.dialog.visible = true;
      this.dialog.title = title;
      this.dialog.name = name;
      this.dialog.injectData = injectData;
      this.dialog.width = width;
      this.dialog.height = height
      this.dialog.top = top
      this.cachedHeight = height
      this.componentProps = componentProps
      this.dialog.isShowFullScreenBtn = isShowFullScreenBtn;
      this.dialog.isFullScreen = isFullScreen;
      this.setDialogHeight()
      this.createFullScreenButton();
    },
    update({component,name,title,isCache,params}){
      if (!name){
        console.warn("miniRouter","动态指定挂载组件时必须提供 name")
        return
      }
      if (name === this.dialog.name){
        console.warn("miniRouter","禁止重复渲染同一个组件")
        return
      }
      /* 更新当前弹窗组件记录的动态 component name */
      this.dialog.name = name
      this.dialog.title = title
      /* 这里可能会是一个坑点 */
      if(!(isCache !== false)){
        !this.unCachedView.includes(component.name) && this.unCachedView.push(component.name)
      }
      /* 若有参数，则携带 */
      if(params){
        this.miniRouteParams = params
      }
      this.dialog.component = component
      this.dialog.params
    },
    getCurrentComponentName(){
      return this.dialog.name
    },
    onCustomEvent(eventName,value){
      this.$emit(eventName,value)
    },
    setDialogHeight(){
       this.$nextTick(() => {
          const elDialogElm = document.querySelectorAll(`.${this.dialogClassName}-class`)[0]
          elDialogElm.style.height = this.dialog.height
        })
    },
    close(isRefresh) {
      const { getRouterRecords,back } = useMiniRouter()
      const routerHistory = getRouterRecords()
      if (routerHistory.length > 1){
        back()
        return
      }
      this.dialog.component = null;
      this.dialog.visible = false;
      this.dialog.title = "";
      this.dialog.injectData = undefined;
      this.dialog.isShowFullScreenBtn = true;
      this.dialog.isFullScreen = false;
      //this.dialog.height = "calc(100vh - 20vh)";
      this.cachedHeight = this.dialog.height;
      const elDialogElm = (document.querySelectorAll(`.${this.dialogClassName}-class`)[0])
      elDialogElm.style.height = this.cachedHeight

      this.$emit("close",isRefresh)
    },
    fullScreenSwitch() {
      this.dialog.isFullScreen = !this.dialog.isFullScreen;
    },
    setClassName() {
      this.dialogClassName = `dialog-view-wrapper-${this._uid}`;
    },
    createFullScreenButton() {
      if (
        !this.dialog.isShowFullScreenBtn ||
        document.getElementById(`fullscree-button-${this._uid}`)
      ) {
        return;
      }
      this.$nextTick(() => {
        const fullButton = document.createElement("button");
        fullButton.innerHTML = this.svgIcon
        fullButton.classList.add("svg-btn");
        fullButton.setAttribute("id", `fullscree-button-${this._uid}`);
        const id = `#dialog-view-wrapper-${this._uid}`;
        const header = document.querySelectorAll(
          `${id} .el-dialog__header`
        )[0];
        header && header.appendChild(fullButton);
        fullButton.addEventListener("click", () => {
          this.fullScreenSwitch();
        });
        this.watchFullScreenState()
      })
    },
    watchFullScreenState(){
      const btnElem = document.getElementById(`fullscree-button-${this._uid}`)
      const elDialogElm = (document.querySelectorAll(`.${this.dialogClassName}-class`)[0])
      if(btnElem){
        this.$watch('dialog.isFullScreen',(newVal) => {
          btnElem.innerHTML = this.svgIcon

          if(!this.dialog.isFullScreen){
            elDialogElm.style.marginTop = "10vh"
            elDialogElm.style.height = this.cachedHeight
          } else {
            elDialogElm.style.height = "100vh"
            elDialogElm.style.marginTop = "0px"
          }
        },{ immediate: true })
      }
    }
  },
  render(h){
    const titleSlot = h(
      "span",
      {
        slot:"title",
        "class":{
          "el-dialog__title":true
        },
        domProps:{
          innerHTML: this.dialog.title
        }
      }
    )

    const componentWrapper = h(
      this.dialog.component,
      {
        props:{
          injectData:this.dialog.injectData,
          ...this.componentProps,
          miniRouteParams: this.miniRouteParams
        },
        on:{
          dispatchCustomEvent:this.onCustomEvent,
          close:this.close
        }
      }
    )
    const keepAlive = h(
      "keep-alive",
      {
        props:{
          exclude: this.unCachedView
        }
      },
      [componentWrapper]
    )
    const elDialogWrapper = h(
      'el-dialog',
      {
        ref: "dialog-wrapper",
        props:{
          center:true,
          top:this.dialog.top,
          fullscreen: this.dialog.isFullScreen,
          title: this.dialog.title,
          visible: this.dialog.visible,
          width: this.dialog.width,
          "custom-class":`${this.dialogClassName}-class`,
          "append-to-body": true,
          "close-on-click-modal": false,
          "close-on-press-escape": true,
          "lock-scroll": true,
        },
        on:{
          close:this.close
        },
        attrs:{
          id: this.dialogClassName
        }
      },
      [
        titleSlot,
        keepAlive,
        this.$slots.footer
      ]
    )

    if(this.dialog.visible){
      return elDialogWrapper
    } else {
      return null
    }
  }
};
</script>
<style scoped lang="scss">
@import "./index.scss"
</style>
