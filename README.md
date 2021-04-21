# ImageFramePlayer
序列帧图片播放插件，支持通过 `canvas` 或者 `<img>` 播放，可控制播放速度，可循环播放、倒序播放，设置循环播放及监听事件等功能。

## fork 说明

该仓库修改自 [vFramePlayer](https://github.com/vmllab-js/vFramePlayer) ，将 `vFramePlayer` 从全局依赖改为模块化引入。

## 示例

https://vmllab-js.github.io/vFramePlayer/

## 如何使用

[npm](https://www.npmjs.com/package/image-frame-player) 下载安装
```shell script
npm i image-frame-player --save
 ```

引入组件

```js
import ImageFramePlayer from 'image-frame-player';
 ```

实例化 `Index`

```js
//将所有图片放入一个数组
const imgArr = ["img/0.jpg","img/1.jpg","img/2.jpg"];

//实例化vFramePlayer
const imageFramePlayer = new ImageFramePlayer({
    dom: document.getElementById("framePlayer"),
    imgArr: imgArr
});
```

## Options
| Field           | Type            | Default  | Description                           | 
| --------------- |:---------------:| :------: | ------------------------------------  |
| `dom`           | `object`        | `body`   | 用于存放图片和 CANVAS 的 DOM 节点，该项必选。 |
| `imgArr`        | `array`         | none     | 图片序列数组，该项必选。支持图片地址及 base64。           |
| `fps`           | `number`        | `25`     | 设置动画播放每秒显示帧频，该项可选。       |
| `useCanvas`     | `boolean`       | `true`   | 是否用CANVAS播放动画，该项可选。如果设置为`false`，则使用 IMG 播放。|
| `loop`          | `number`        | `0`      | 循环播放次数，该项可选。不设置则不循环播放，`-1`为无限循环。|
| `yoyo`          | `boolean`       | `false`  | 配合`loop`使用，该项可选。如果设置为`true`，循环播放的时候会回播。|

```js
//示例
var imageFramePlayer = new ImageFramePlayer({
    dom : document.getElementById("framePlayer"),
    imgArr : imgArr,
    fps : 30,
    useCanvas : false,
    loop : 10,
    yoyo : true
});
```
## Methods
实例化完成后，你可以使用以下方法进行播放序列图动画：

| Field           | Parameter              | Description                         | 
| --------------- | :--------------------: | ----------------------------------- |
| `play()`        | `start` `end` `options`  | 播放序列图动画，参数见下表。 |
| `goto()`        | `i`                    | 直接跳到第 `i` 帧，`i` 必选。type:`number`。 |
| `pause()`       | none                   | 暂停播放动画。|
| `stop()`        | none                   | 停止播放动画，重置数据。|
| `destroy()`     | none                   | 清除所有动画及监听事件。|
| `get()`         | `attr`                 | 获取参数值。可获取参数同 [Options](#options) Field列。type:`string`。 |
| `set()`         | `attr`                 | 设置参数值。可设置参数同 [Options](#options) Field列。type:`string`。 |

`play()` <span id="play">方法参数：</span>

| Field           | Type        | Default      | Description           | 
| --------------- | :---------: | ------------ |---------------------- |
| `start`         | `number`    | `0`          | 播放开始帧，该项可选。   |
| `end`           | `i`         | last         | 播放结束帧，该项可选。如果end大于start，则倒序播放。   |
| `options`       | `object`    | none         | 播放参数，该项可选。同[Options](#options)。  |

`play()`方法`options`其他参数设置：
- `onComplete()`- 播放完成时执行的方法，该项可选；
- `onUpdate(frame,times,asc)` - 播放过程中执行的方法，该项可选。
    - `frame` - 当前帧。
    - `times` - 已播放次数。
    - `asc` - 是否升序播放。

```js
//示例
imageFramePlayer.play(10,100,{
    yoyo:true,
    fps:30,
    loop:10,
    onComplete: ()=> {
        console.log("播放完成");
    },
    onUpdate: (frame,times,asc)=>{
        console.log("当前播放第"+ frame +"帧");
        console.log("已经循环播放"+ times +"次");
        console.log("当前是否是升序播放："+ asc);
    }
})
```
## Events
播放事件的监听及取消监听的方法。

| Field           | Parameter          | Description           | 
| --------------- | :----------------: |---------------------- |
| `on()`          | `events` `handler`  | 监听事件。`events` - 监听事件名称，`handler` - 监听事件执行方法。   |
| `one()`         | `events` `handler`  | 监听一次事件。参数同上。   |
| `off()`         | `events` `handler`  | 结束监听。参数同上。   |

事件监听名称：
- `"play"` - 开始播放
- `"pause"` - 暂停动画
- `"stop"` - 停止动画
- `"update"` - 动画播放过程中

```js
//示例
imageFramePlayer.on("play",function () {
    console.log("开始播放");
})
```
