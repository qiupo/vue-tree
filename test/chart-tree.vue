<template>
  <div class="area_wrapper">
    <vue-tree
      v-if="rootData"
      ref="rootTreeNodeRef"
      :key="rootData.id"
      :tree-node-data="rootData"
      isRoot
    >
      <template v-slot:default="slotProps">
        <div class="parent_wrapper">
          <!-- <div class="parent_content" @click="handlerNodeClick(slotProps.data)"> -->
          <div>
            <p class="content_name">{{ slotProps.data.treeNodeData.name }}</p>
            <table class="content_name" cellspacing="0">
              <tr>
                <td>First name</td>
                <td>Last name</td>
              </tr>
              <tr>
                <td>John</td>
                <td>Doe</td>
              </tr>
              <tr>
                <td>Jane</td>
                <td>Doe</td>
              </tr>
            </table>
            <button v-if="slotProps.data.treeNodeData.children.length!==0" class="content_name" @click="al(slotProps.data)">
              展开下一级
            </button>
          </div>
        </div>
      </template>
    </vue-tree>
  </div>
</template>
<script>
import VueTree,{updatePartTree} from '../lib/vueTree.umd.min.js';
export default {
  components: {
    "vue-tree": VueTree,
  },
  data() {
    return {
      rootData: genTestData(),
      newName: "",
      activeNode: null,
      nameModalVisible: false,
    };
  },
  methods: {
    handlerNodeClick(activeData) {
      this.activeData = activeData;
      this.newName = activeData.treeNodeData.name;
      this.changeNameModal();
    },
    changeNameModal() {
      this.nameModalVisible = !this.nameModalVisible;
    },
    updateName() {
      this.activeData.treeNodeData.name = this.newName;
      // 节点高度改变，需要调用 updatePartTree 方法进行位置的重新计算
      updatePartTree(this.activeData.$treeNodeRefs.treeNodeRef);
      this.changeNameModal();
    },
    al(slotProps) {
      console.log(slotProps);
      slotProps.treeNodeData.isOpen = !slotProps.treeNodeData.isOpen;
      updatePartTree(slotProps.$treeNodeRefs.treeNodeRef);
    },
  },
};
let id = 0;
// 用于生成测试数据
/**
 * 用于生成测试数据
 * @param maxHeight 树状图的最大深度
 * @param maxChildrenHeight 节点最多拥有的直接子元素个数
 * @param height 当前路径的深度
 */
function genTestData(maxHeight = 5, maxChildrenLen = 5, height = 0) {
  const rootNode = {
    id: id++,
    name: genTestName(),
    isOpen:false,
    children:[]
  };
  if (height + 1 === maxHeight) {
    return rootNode;
  }
  rootNode.children = Array(Math.round(Math.random() * maxChildrenLen))
    .fill(0)
    .map(() => {
      return genTestData(maxHeight, maxChildrenLen, height + 1);
    });
  return rootNode;
}

function genTestName() {
  const nameLen = Math.random() * 10 + 10;
  let name = "";
  while (name.length < nameLen) {
    name += Math.random().toString(16).slice(2);
  }
  return name.slice(0, nameLen);
}
</script>
<style scoped lang="less">
td {
  border: solid #add9c0;
  border-width: 0px 1px 1px 0px;
}

table {
  border: solid #add9c0;
  border-width: 1px 0px 0px 1px;
}

.area_wrapper {
  min-height: 372px;
  box-sizing: border-box;
  padding: 60px;
  overflow-x: scroll;
  border-radius: 4px;
  background-color: #f5f5f5;
}

.parent_wrapper {
  display: inline-block;
  width: 200px;
  padding: 16px;
  box-sizing: border-box;
  /* border-left-width: 3px;
        border-left-style: solid;
        border-color: #F65656; */
  border-radius: 4px;
  cursor: pointer;
  overflow: hidden;
  background-color: #fff;
  transition: all 0.15s ease;

  &:hover {
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
  }
}

.parent_content {
  display: flex;
  align-items: flex-start;
}

.content_name {
  flex: 1;
  line-height: 16px;
  margin: 0;
  margin-left: 10px;
  overflow: hidden;
  white-space: initial;
  word-break: break-all;
  font-size: 14px;
  color: #333;
}

.content {
  position: fixed;
  top: 50%;
  left: 50%;
  padding: 20px 30px;
  transform: translate(-50%, -50%);
  background-color: #fff;
  z-index: 200;
}

.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.65);
  z-index: 100;
}
</style>