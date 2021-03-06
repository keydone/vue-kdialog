
# vue-kdialog
> A Vue dialog component
> 基于 vue 2.x 版本的弹窗组件

![](https://raw.githubusercontent.com/keydone/vue-kdialog/master/screenshot/confirm.png)

<img src="https://raw.githubusercontent.com/keydone/vue-kdialog/master/screenshot/small.gif" width="400px" />

## Usage (使用)

1, import kdialog component (导入 kdialog 组件);

2, vue.use(kdialog) (调用vue.use(kdialog)方法);

3, congratulations! (好了，可以尽情使用了！)

```javascript
import Vue from 'vue';
import kdialog from 'vue-kdialog';

Vue.use(kdialog);
```

## Suport (功能)

> There are many types u can use (内置了多种功能供你使用):

1, $loading, need to close manually (需要手动调用关闭方法，否则不会消失):
```javascript
const loading = this.$loading();
// close it when u need:
loading.close();
```

2, $toast, it will disappear after default 2500ms, change it like this (无需手动调用关闭方法，在参数timer时间后自动消失，默认为2500ms):
```javascript
this.$toast({
	msg: 'This is a test for toast',
	timer: 3000
});
```

3, $alert, there is only one button (只有1个确定按钮):
```javascript
this.$alert({
	width: 300,
	title:'Alert',
	content:'<p style="text-align:center">This is a test for Alert</p>',
	okText:'Yes'
});
```

4, $confirm, there a 2 buttons (有2个按钮):
```javascript
this.$confirm({
	width: 500,
	content:'<p style="text-align:center">This is a test for Alert</p>',
	okText:'Yes'
});
```

5, component, 自定义组件
```javascript
import a from 'a'
this.$confirm({
	width: 500,
	component: a,
	ok(kdialog){
		kdialog.disabled = true;
	},
	cancel(kdialog){
		kdialog.close();
	}
});
```

参数列表：

params    | type | description (参数解释) |
--------- | -------- | -------- |
_body_ | reserved words(不要覆盖) | control the position of the dialog (弹窗唤起位置class) |
_rollfrom_ | String ('center'/'top'/'bottom' default : 'center') | control the position of the dialog (弹窗唤起位置class) |
title    | String | title of dialog (弹窗标题) |
show  | Boolen | show or remove the dialog dom (显示或移除弹窗dom节点) |
customClass | String | custom className for dialog (弹窗自定义className) |
_type | reserved words(不要覆盖) | don't override it (内部组件使用，请勿覆盖)|
_runtime | reserved words (default : 'pc' 不要覆盖) | kdialog.setRunTime('m'); (用于判断运行环境进行样式自定义，例如pc/m)|
title    | String| title of dialog (弹窗标题) |
width    | Number/String (width unit) | width of dialog (弹窗宽度) |
content  | String | content of dialog (弹窗内容) |
ok  | function | callback for ok botton/toast (确定按钮/toast回调) |
cancel  | function | callback for cancel button (取消按钮回调) |
component | Object | custom component object (自定义组件对象) |
okEvent | Object | custom ok event bus name (自定义确定 event bus 名称) |
cancelEvent | Object | custom cancel event bus name (自定义取消 event bus 名称) |
disabled | Boolen | disable click the confirm (禁用确认按钮点击事件) |
submiting | Boolen | submiting style for confirm button (确认按钮正在提交样式) |
shadowClose | Boolen (default : true) | click shadow to close dialog (点击背景遮罩关闭弹窗) |
okText  | String | confirm button text for alert (alert确认按钮文案) |
confirmText  | String | confirm button text for confirm (confirm确认按钮文案) |
cancelText  | String | cancel button text (取消按钮文案) |
timer | Number | show toast time (显示toast时长) |
msg | Number | show toast msg (显示toast消息) |
stopBodyScroll | Boolen(default: true) | stop Body Scroll, support pc & m (禁止body跟随弹窗内容区域滚动而滚动的默认行为, 同样支持移动端) |

> By the way:

u can use it like this (你可以这样引入):

1) no css
```javascript
import Vue from 'vue';
import kdialog from 'vue-kdialog';
Vue.use(kdialog);
```

2) with postcss
```javascript
import Vue from 'vue';
import kdialog from 'vue-kdialog';
import kdialog from 'vue-kdialog/src/keydialog_pc.css';
Vue.use(kdialog);
```

3) event bus & callback (内置了 event bus 和回调函数)
There 2 ways to handle confirm & cancel button:

① event bus:
```javascript
import Vue from 'vue';
import kdialog from 'vue-kdialog'
Vue.use(kdialog);
kdialog.eventbus();
// init the eventbus, the bus will be mounted to Vue prototype (全局使用eventbus事件，并挂载到vue原型上)

// when u need:
const this = vm;
vm.$bus.$off('changeAct'); // !!!!off event first!!!! of u will get more emit
vm.$confirm({
	title: '<h2>title with html</h2>'
	okEvent: 'changeAct',  // emit ok event, click the ok, it will emit 'changeAct'
	cancelEvent: 'cancelChangeAct',	// emit cancel event, click the ok, it will emit 'changeAct'
	componentData: {
		data:'something'
	}
});

vm.$bus.$on('changeAct', (data)=>{
	console.log(data);  // the dialog object
	console.log(data.$dialog.componentData); // {data:'something'}
	data.$dialog.close();
	//after close, the dialog, 'changeAct' & 'cancelChangeAct' events will be destroyed automatically, so please use different event name for every different dialog. 
	//(执行close方法后，当前弹窗实例和相应 event bus 事件会被自动销毁。所以请给不同的弹窗使用不同的 event 事件名称)
});

```

② callback
```javascript
vm.$alert({
	title: 'gift'
	ok($dialog){  // ok callback
		console.log($dialog); // the $alert dialog itself;
	},
});
```

## Tips (提醒)
1, Using it only u know it deeply, becourse u may receive multiple events. when event bus & callback use at same time, the callback will be priority.

1, 如果你不能熟练使用 event bus， 最好不要使用，因为使用不当可能会接收到多次提交事件。另外，当 event bus 和回调同时使用时，优先处理回调，所以你可以在开启 event bus 时按需使用回调来处理事务。

2, When building, getting error with webpack.optimize.uglifyjsplugin config, Using uglifyjs-webpack-plugin to instead of webpack.optimize.uglifyjsplugin, it should be fine.

2, 如果使用 webpack.optimize.uglifyjsplugin 打包过程中出现弹窗组件打包报错， 请换成 uglifyjs-webpack-plugin 去压缩你的 JS 文件， 应该就不会报错了。

# API document (开发文档)

more to see: https://keydone.github.io/vue-kdialog/dist/examples.html

##1.0.3 new:
* fix: bug in IE10.

##1.0.0 new:
* stopBodyScroll for mobile is back since this version.

In pc, u just need add max-height, overflow-y:auto for dialog's content;

But, in mobile, u must put scroll content in 'kdialog_scroll_wrap', and add max-height;

##0.7.14 new:
* import kdialog from 'vue-kdialog', but no css;
* import 'vue-kdialog/src/keydialog_pc.css'; // browsers support: ["last 20 versions", "ie >=9"];(or scss)
* Add set language: kdialog.setLanguage('en'); // only can be 'en'/'zh-cn'; default: 'zh-cn';
* or kdialog.setLanguage({ ok: 'OK', confirm: 'Confirm', cancel: 'cancel' });

* remove toastName & modalName, Add 'modal_enter', 'modal_leave' for animation;

##0.7.4 new:
* stopBodyScroll: to stop body's scroll action while scrolling dialog content. (滚动弹窗内容阻止body滚动)

> To be continued !  ( It will be strong )

> 未完待续。。。 ( 后续版本会更加强劲 )
