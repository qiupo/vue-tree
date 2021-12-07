<template>
  <div id="area">
    <vue-tree
      v-if="treeNodeData"
      ref="rootTreeNodeRef"
      :key="treeNodeData.id"
      :tree-node-data="treeNodeData"
      isRoot
    >
    </vue-tree>
  </div>
</template>
<script>
import VueTree from "./vueTree.vue";
export default {
  components: {
    VueTree,
  },
  name: "tree",
};
window.onload = function () {
  var oDiv = document.getElementById("area"); // 获取到要拖拽的元素
  drag(oDiv); // 调用自己封装的拖拽函数

  function drag(obj) {
    obj.onmousedown = function (e) {
      var e = e || window.event; // 兼容 IE
      // 鼠标点击物体那一刻相对于物体左侧边框的距离=点击时的位置相对于浏览器
      // 最左边的距离-物体左边框相对于浏览器最左边的距离，纵向同理
      var divX = e.clientX - this.offsetLeft;
      var divY = e.clientY - this.offsetTop;
      let y = oDiv.offsetTop;
      let x = oDiv.offsetLeft;
      if (obj.setCapture) {
        obj.setCapture(); // 修复低版本 IE bug
      }
      document.onmousemove = function (e) {
        obj.style.cursor = "pointer";
        var e = e || window.event;

        var disX = e.clientX - divX;
        var disY = e.clientY - divY;
        // 移动时重新得到物体的距离，解决拖动时出现晃动现象
        oDiv.style.top = y + disY + "px";
        oDiv.style.left = x + disX + "px";
        document.onmouseup = function () {
          // 鼠标抬起时不再移动
          // 预防鼠标弹起来后还会循环（即预防鼠标放上去的时候还会移动）
          document.onmousedown = document.onmousemove = null;
          if (obj.releaseCapture) {
            obj.releaseCapture(); // 修复低版本 IE bug
          }
        };
      };
    };
  }
};
</script>

<style>
.area {
  box-sizing: border-box;
  overflow: hidden;
  border-radius: 4px;
  position: absolute;
}
</style>