### 2021-07-05 v0.2.3-beta.2 3 4 5
- 移除
  - 移除 setDestroyCallBack 方法，不应该在该组件销毁时调用，否则会引起本次注册点弹窗再次打开时找不到组件 beta2
  - miniRouter 更改导出模式

- 新增 update 方法接受 title 解构，用于动态更新标题
### 2021-07-05 v0.2.3-beta.1
- 增量非破坏性升级
  - 本次peerDep 预依赖miniRouter包，获取状态，在close方法中判断是否处于二层+ 弹窗，如果是则调用back，否则直接close
### 2021-05-16 v0.2.3
- 新增
    - getCurrentComponentName 配合miniRouter获取组件命名 - 路由唯一名称
    - setDestroyCallBack 配合miniRouter，在弹窗销毁时清空 miniRouter模块变量的值

- 更新
    - update 新增 解构name参数，用于保存路由唯一名称，并可被getCurrentComponentName获取
    - update 内新增参数校验
### 2021-05-15 v0.2.2
- 新增
    - dispatchCustomEvent 去掉了 er ，动名词命名的错误，兼容并包 dispatcherCustomEvent，但是会在以后的版本删掉
### 2021-05-15 v0.2.1
- 新增
  - 新增 update 方法，赋予组件动态渲染的能力
  - 新增 keep-alive 组件包裹，赋予动态组件渲染后的缓存能力
  - 结合 miniRouter 使用，赋予组件弹窗内路由的能力
### 2021-04-05 v0.2.0
- 新增
  - 使用render函数重写 template
  - 新增options.componentProps，在合适的场景下可直接替代 injectData传参数给子组件。
### 2021-03-19 v0.1.13
- 修复
  - 默认 isFullScreen 时，dialog全屏高度不正常
### 2021-03-15 v0.1.12
- 新增
  - 支持 margin-top 
### 2021-03-06 v0.1.11
- 新增
  - 新增options属性title， 提供渲染 html，使用 v-html 方式，支持方式比较单一
### 2021-02-17 v0.1.11-beta.1
- 修复
  - bugfix: 触发多次回调事件(close),上一次的修复在某个场景下仍然会触发两次（再次修复）
  - 关键点：去除 $listeners 无脑emit，使用新增的中间事件修复 dispatcherCustomEvent
- 新增 
   - dispatcherCustomEvent事件  
### 2021-02-17 v0.1.10
- 修复
  - bugfix: 触发多次回调事件(close)
### 2021-01-21 v0.1.9
- 修复
  - 弹窗关闭时使用 v-if 销毁所有实例
### 2021-01-19 v0.1.8
- 修复
  - 在setDialogHeight内部调用nextTick判断条件有误，去掉判断条件。
### 2021-01-19 v0.1.7
- 修复
  - dialog-view 实例中的状态在close阶段不会初始化的问题，导致第二个弹窗继承了第一个弹窗关闭时高度的问题。
### 2021-01-18 v0.1.6
- 修复
  - 全屏缩小状态的 btn图标
### 2021-01-08 v0.1.5
- 修复
  - 关闭dialog时（如果此时dialog是全屏） 忘记重置高度的bug
### 2021-01-07 v0.1.4
- 修复
  - el-dialog-body 的高度固定
### 2021-01-07 v0.1.3
- 修复
  - 全屏操作时距离顶部的边距无法消除 通过 !important 关键词已解决
### 2021-01-05 v0.1.2
- 新增
  - 可设置dialog宽度,对应options属性为 height
### 2020-12-31 v0.1.1
- 修复
  - 环境不同的情况下css样式约束、另外修正options中的属性名称