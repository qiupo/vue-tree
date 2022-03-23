<template>
  <!-- data-opened， 0(opened) 表示没被展开过，1(noOpened) 代表已经被展开过了（或者没有子元素所以没有展开不展开的概念，直接认为是1） -->
  <div
    :class="treeNodeClassName"
    ref="treeNodeRef"
    :data-opened="dataOpened.noOpened"
  >
    <div :class="treeParentClassName" :id="treeNodeData.id"> 
      <div class="stretch_node_r" v-if="!isRoot">
        <p class="line_r"></p>
      </div>
      <div class="node_content">
        <slot v-bind:data="{ treeNodeData, $treeNodeRefs: $refs }"></slot>
      </div>
    </div>
    <!-- 控制展开/收缩的节点 -->
    <div :class="stretchNodeClassName" v-if="childrenLen">
      <p class="line_l"></p>
      <p
        :class="['line_dot', treeNodeData.isOpen ? 'cut_dot' : 'add_dot']"
        @click="changeOpen"
      ></p>
    </div>
    <div
      :class="treeChildrenClassName"
      v-if="childrenLen"
      v-show="treeNodeData.isOpen"
    >
      <p :class="connectLineClassName" v-if="childrenLen > 1"></p>
      <vue-tree
        v-for="item in treeNodeData.children"
        :key="item.id"
        :treeNodeData="item"
        :isRoot="false"
      >
        <template v-slot:default="slotProps">
          <slot v-bind:data="slotProps.data"></slot>
        </template>
      </vue-tree>
    </div>
  </div>
</template>
<script>
import {
  dataOpened,
  treeParentClassName,
  treeChildrenClassName,
  treeNodeClassName,
  stretchNodeClassName,
  connectLineClassName,
} from "./const";
import { resetTree, updatePartTree } from "./util";

export default {
  name: "vueTree",
  props: {
    // 当前节点是否是根节点
    isRoot: {
      type: Boolean,
      required: false,
      default: true,
    },
    // 当前节点的数据信息
    treeNodeData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      // treeNode 节点是否已经被展开的 data- 标识
      dataOpened,
      // className 类名
      treeParentClassName,
      treeChildrenClassName,
      treeNodeClassName,
      stretchNodeClassName,
      connectLineClassName,
    };
  },
  computed: {
    childrenLen() {
      return (this.treeNodeData.children || []).length;
    },
  },
  mounted() {
    if (this.isRoot) {
      console.log(this.$refs.treeNodeRef);
      resetTree(this.$refs.treeNodeRef);
    }
  },
  methods: {
    changeOpen() {
      this.treeNodeData.isOpen = !this.treeNodeData.isOpen;
      updatePartTree(this.$refs.treeNodeRef);
    },
  },
};
</script>
<style lang="less" scoped>
@transionDuration: 0.15s;
.tree_node {
  position: relative;
  margin-bottom: 34px;
  font-size: 0;
  white-space: nowrap;
  &:last-child {
    margin-bottom: 0;
  }
  .tree_parent {
    position: relative;
    vertical-align: top;
    display: inline-block;
  }
  @stretchLineW: 30px;
  @stretchNodeH: 20px;
  @stretchLineL: 49px;

  .stretch_node_r {
    display: inline-block;
    width: @stretchLineL;
    .line_r {
      position: absolute;
      left: @stretchNodeH - (@stretchLineL + @stretchNodeH - 4) / 2;
      top: 50%;
      width: @stretchLineW + (@stretchLineL + @stretchNodeH - 4) / 2;
    }
  }
  .node_content {
    display: inline-block;
  }

  .stretch_node {
    position: relative;
    display: inline-block;
    z-index: 10;
    p {
      display: inline-block;
      vertical-align: middle;
    }
    .line_dot {
      width: @stretchNodeH;
      height: @stretchNodeH;
      cursor: pointer;
      user-select: none;
      border-radius: 50%;
      background-size: contain;
      background-position: 50%;
      background-repeat: no-repeat;
      background-color: #8a8a8a;
      transition: all @transionDuration;
      &.add_dot {
        background: url("data:image/svg+xml,%3Csvg width='22' height='24' viewBox='0 0 22 24' xmlns='http://www.w3.org/2000/svg'%3E%3Cdefs%3E%3Cfilter x='-60%25' y='-60%25' width='220%25' height='220%25' filterUnits='objectBoundingBox' id='a'%3E%3CfeGaussianBlur stdDeviation='2' in='SourceGraphic'/%3E%3C/filter%3E%3C/defs%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cpath d='M1 .612h20v20H1z'/%3E%3Cpath d='M14.808 7H5.192A.192.192 0 0 0 5 7.192v9.616c0 .106.086.192.192.192h9.616a.192.192 0 0 0 .192-.192V7.192A.192.192 0 0 0 14.808 7z' fill='%23001E94' filter='url(%23a)' transform='translate(1 .612)'/%3E%3Cpath d='M17.25 4.112H4.75a.25.25 0 0 0-.25.25v12.5c0 .138.112.25.25.25h12.5a.25.25 0 0 0 .25-.25v-12.5a.25.25 0 0 0-.25-.25z' fill='%23FFF'/%3E%3Cpath d='M14.5 10.862a.25.25 0 0 1-.25.25h-6.5a.25.25 0 0 1-.25-.25v-.5a.25.25 0 0 1 .25-.25h6.5a.25.25 0 0 1 .25.25v.5z' fill='%23323F83'/%3E%3C/g%3E%3C/svg%3E");
        background-size:cover
      }
      &.cut_dot {
        background: url("data:image/svg+xml,%3Csvg width='22' height='24' viewBox='0 0 22 24' xmlns='http://www.w3.org/2000/svg'%3E%3Cdefs%3E%3Cfilter x='-60%25' y='-60%25' width='220%25' height='220%25' filterUnits='objectBoundingBox' id='a'%3E%3CfeGaussianBlur stdDeviation='2' in='SourceGraphic'/%3E%3C/filter%3E%3C/defs%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cpath d='M1 .612h20v20H1z'/%3E%3Cpath d='M14.808 7H5.192A.192.192 0 0 0 5 7.192v9.616c0 .106.086.192.192.192h9.616a.192.192 0 0 0 .192-.192V7.192A.192.192 0 0 0 14.808 7z' fill='%23001E94' filter='url(%23a)' transform='translate(1 .612)'/%3E%3Cpath d='M17.25 4.112H4.75a.25.25 0 0 0-.25.25v12.5c0 .138.112.25.25.25h12.5a.25.25 0 0 0 .25-.25v-12.5a.25.25 0 0 0-.25-.25z' fill='%23FFF'/%3E%3Cpath d='M14.5 10.862a.25.25 0 0 1-.25.25H11.5v2.75a.25.25 0 0 1-.25.25h-.5a.25.25 0 0 1-.25-.25v-2.75H7.75a.25.25 0 0 1-.25-.25v-.5a.25.25 0 0 1 .25-.25h2.75v-2.75a.25.25 0 0 1 .25-.25h.5a.25.25 0 0 1 .25.25v2.75h2.75a.25.25 0 0 1 .25.25v.5z' fill='%23323F83'/%3E%3C/g%3E%3C/svg%3E");
        background-size:cover
      }
    }
  }
  .line_l {
    position: relative;
    left: 3px;
    display: inline-block;
    vertical-align: middle;
    width: @stretchLineW;
    height: 2px;
    background:linear-gradient(to right,#D1E2FF,#4686F2);
  }
  .line_r {
    display: inline-block;
    vertical-align: middle;
    width: @stretchLineW;
    height: 2px;
    background:linear-gradient(to right,#4686F2,#D1E2FF);
  }
  .tree_children {
    position: relative;
    display: inline-block;
    vertical-align: top;
    .connect_line {
      position: absolute;
      left: @stretchNodeH - (@stretchLineL + @stretchNodeH - 4) / 2;
      top: 0;
      height: 100%;
      width: 2px;
      background: linear-gradient(#D1E2FF, #4686F2);
      z-index: 6;
    }
  }
}
</style>
