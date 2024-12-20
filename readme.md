## 组件说明
适用项目：uniapp
用途：数字键盘
数字键盘，支持自定义关闭和删除按钮，随机数字键盘

## 引入组件
`import yKeyboard from '@/uni_modules/y-keyboard/y-keyboard.vue'`
### 注册组件
```js
components: {
    yKeyboard
},
// 调用组件
<y-keyboard
	@input="keyInput"
	@delete="deleteFun"
	@longpress="butLongpress"
	ref="yKeyboardRef">
</y-keyboard>

// 使用方法
data() {
	return {
		key1: "",
	}
},

methods: {
    // 长按事件
	butLongpress() {
		this.key1 = "";
	},
	deleteFun() {
		if(this.key1.length == 0) return
		this.key1 = this.key1.slice(0, -1);
	},
	keyInput(val) {
		this.key1 += val;
	},
	openK() {
		this.$refs.yKeyboardRef.show();
	},
}
```
## 属性说明

|名称|类型|是否必填|默认值|可选值|说明|
|----|----|----|----|----|----|
|random|Boolean|否|false|true/false|是否随机键盘|
|openRandom|Boolean|否|false|true/false|是否每次打开键盘都随机|


## 方法

|事件名称|说明|返回参数|
|----|----|----|
|input|用户点击键盘数字时触发|返回点击的数字|
|delete|点击点击删除按钮时触发|无|
|longpress|用户长按删除按钮时触发|无|


## 演示案例
可下载示例项目查看