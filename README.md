# vueTree
`vueTree` 是一个基于vue的可自定义的折线树图形图组件；
### demo
拉取项目后安装依赖并运行npm run serve;访问[http://localhost:8080/](http://localhost:8080/)即可
## 安装
```
npm install vueTree --save
```
## 导入
```js
import Vue from 'vue'
// in ES6 modules
import VueTree, { resetTree, updatePartTree }  from 'vueTree'

// in CommonJS
const VueTree = require('vueTree')

export default {
  components: {
    VueTree
  }
}
```

## 方法
`VueTree` 即为组件本身；
当开发者自行定义的 `slot`节点发生变化时（例如位置、尺寸等发生改变），由开发者主动调用 `updatePartTree` 进行树状图结构的更新；
如果因为特殊的操作需要重置整个树状图，则可以调用 `resetTree` 完成, `resetTree`接收一个 `VueTree`节点作为参数（可以通过 `$ref`获取）

## Options

### Props

|参数|类型|描述|默认值|是否必须|
|----|---|----|----|---|
|`isRoot`|`Boolean`|是否是整个树状结构的根节点（即 `rootNode`）|`true`|否|
|`treeNodeData`|`Object`|当前节点的数据信息|-|是|

#### treeNodeData

|参数|类型|描述|默认值|是否必须|
|----|---|----|----|---|
|`children`|`Array<treeNodeData>`|当前节点的子节点|-|否|
|`isOpen`|`Boolean`|当前节点是否是展开(`open`)状态|-|否|
|`other`|`any`|当前节点相关的其他值，这些值由开发者决定，一般来说是可能会被 `slot` 使用的值|-|否|
