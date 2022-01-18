#  dialog-view 组件

声明式导入dialog，服务形式的API调用方式

暂时未发npm包，如果你需要这个功能可以直接fork，记得给个 🌟

## 安装

``` 
npm i dialog-view
```



### 局部

```vue
<template>
    <div>
        <dialog-view ref="dialog">
            <div slot="footer">666</div>
        </dialog-view>
    </div>
</template>

<script>
import { DialogView } from "dialog-view"
export default {
    name: "demo",
    components:{
        DialogView
    },
    mounted(){
        this.open()
    },
    methods: {
        open(){
            this.$refs.dialog.open({
                title:"传统功夫 接、化、发",
                component:null,
                injectData:{

                },
              	componentProps:{}
            })
        }
    }
}
</script>
```



### 全局

```js
import DialogView from "dialog-view"
Vue.use(DialogView);
```



## Event

| 事件名称 | 说明                                                         | 回调参数         |
| -------- | ------------------------------------------------------------ | ---------------- |
| close    | 调用close后或点击关闭按钮后,isRefresh是调用 method > close时提供的入参，默认为false | close(isRefresh) |



## Methods

| 方法名   | 说明       | 参数                          |
| -------- | ---------- | ----------------------------- |
| open( )  | 打开dialog | open(options:object):void     |
| close( ) | 关闭dialog | close(isRefresh:boolean):void |



## Options



| 参数                | 说明                          | 类型    | 默认值    |
| ------------------- | ----------------------------- | ------- | --------- |
| title               | 弹窗标题                      | html string、string |""  |
| component           | 需要渲染的组件                | object  | null      |
| injectData          | 注入的props，传递给 component | any     | undefined |
| width               | 弹窗宽度                      | css string | 80%       |
| isShowFullScreenBtn | 是否显示全屏控制按钮          | boolean | true      |
| isFullScreen        | 是否全屏                      | boolean | rue       |
| height              | 弹窗高度                      | css string | calc(100vh - 20vh) |
| Top | margin-top | css string | 10vh |
| componentProps | 注入的props，直接传递给 component ，而不是接收到injectData上面 | object | null |

## Slot

- Footer - 主要用于更换底部按钮区域 ，比如一个弹窗中的组件 A，被用于查看与修改（A组件被复用），查看的时候默认隐藏底部的按钮区域（提交修改按钮）,修改时可以传入injectData来控制底部的按钮显示



## 解决了什么问题

目前我公司中后台dialog的两种方式：

- mixin 
- 全局注册管理dialog



### 分析（我的理解）

关于方案一：

- mixin 的缺点直接心里过一遍就是这种方式的缺点，（现在连vue官方都不推荐使用mixin）

- template 代码太长，太繁琐

关于方案二：

对于提出的dialogs 组件，显然是能够控制 dialog 的开关的，但是作为dialog ， 本身就是与业务耦合在一起，从现有的文件布局来看，每个独立的业务本身有独立的业务文件夹，业务内的 dialog、自定义特有的 component与业务高度关联，抽离出去的确实可见代码干净清晰，但少了一种关联性，（即在多人协调开发的时候不知道这个页面到底依赖了哪个dialog，结果还是回到 mainjs 找到对应的dialog进行修改，移动，删除等等操作）

> 我在做老城改造的时候迁移项目发现了这种方式的痛点。我有些页面需要迁移，有些不要，此时我还得去全局管理dialog的目录去找到需要的dialog 然后再随业务代码迁移到新项目中

并且如果按照这种方式，集中配置注册dialog的时候dialog的name 不能冲突，无形中又是一个心理压力，来自命名的压力！因为它使用起来是像这样子：

``` js
this.$dialog.open("name",options)  // name 是一个命名心理压力
```

集中式的dialog 管理还会带来一个问题，就是当多人协作开发一个项目的时候，你从开始就不会知道这个SFC文件到底引用了哪一个dialog，因为全在this上通过 name 来打开，结果就是还得去集中管理的目录里去找，你也并不知道本页面会打开一个弹窗，因为在template中你看不到dialog有关的html标签

## 注意事项

如何要在 dialog-view 中监听自定义回调事件请参考以下代码 v0.1.11 版本带来的新特性

```js
// 点击按钮 触发组件提供的dispatchCustomEvent ，事件中心会分发 customEvent 事件，并传入入参值为 true
<button @click="$emit('dispatchCustomEvent','customEvent',true)">

```
> customEvent 可以换成你想监听的事件名称