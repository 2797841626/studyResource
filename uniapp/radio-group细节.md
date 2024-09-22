#### radio-group

单项选择器，内部由多个 `<radio>` 组成。通过把多个`radio`包裹在一个`radio-group`下，实现这些`radio`的单选。

**属性说明**

|属性名|类型|默认值|说明|平台差异说明|
|---|---|---|---|---|
|value|String||`<radio>` 标识。当该 `<radio>` 选中时，`<radio-group>` 的 change 事件会携带 `<radio>` 的 value||
|checked|Boolean|false|当前是否选中||
|disabled|Boolean|false|是否禁用|
例如
  `<radio-group @change="handleBottomFrameCheckChange">`
`<radio value="有问题">有问题</radio>`
`<radio value="没问题">没问题</radio>`
`</radio-group>`

js代码
  `handleBottomFrameCheckChange(value) {`
  `if (value.detail.value == "有问题") {`
  `this.showBottomFrame = true;`
    `} else {`
  `this.showBottomFrame = false;`
    `}`
`},`

radio的value会随着函数传进来，但value并非就是等于号后面的值
而是如下图
![[Pasted image 20240505023953.png]]
所以我们需要写出value.detail.value才行