<template>
  <div
    class="bg-grey"
    style="height: 100%;background:url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTM2NiIgaGVpZ2h0PSI1NzIiIHZpZXdCb3g9IjAgMCAxMzY2IDU3MiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZGVmcz48bGluZWFyR3JhZGllbnQgeDE9IjUuODA3JSIgeTE9IjQxLjI1NSUiIHgyPSI4OS43MzglIiB5Mj0iNDkuNTYxJSIgaWQ9ImEiPjxzdG9wIHN0b3AtY29sb3I9IiNGNkY5RkYiIHN0b3Atb3BhY2l0eT0iMCIgb2Zmc2V0PSIwJSIvPjxzdG9wIHN0b3AtY29sb3I9IiNFMUVDRkQiIG9mZnNldD0iMTAwJSIvPjwvbGluZWFyR3JhZGllbnQ+PC9kZWZzPjxwYXRoIGQ9Ik0xMzY2IDI0OS40ODNMMCA1NzEuMjY4VjBoMTM2NnoiIGZpbGw9InVybCgjYSkiIGZpbGwtcnVsZT0iZXZlbm9kZCIvPjwvc3ZnPg==) no-repeat;background-size: contain;"
  >
    <div id="app" style="height: 100%; display: flex; flex-direction: column" v-cloak>
      <div class="pulse">
        <h-icon color="#5cb85c" name="success_fill"></h-icon>
        <div>
          <span class="text">拓扑图</span>
        </div>
      </div>
      <div id="tree" style="width: 100vw; height: 100vh">
        <div id="area_wrapper">
          <vue-tree
            v-if="rootData"
            ref="rootTreeNodeRef"
            :key="rootData.id"
            :tree-node-data="rootData"
            isRoot
          >
            <template v-slot:default="slotProps">
              <div class="parent_wrapper_hidden">
                <div style="display: flex; flex-direction: row;height: 56px;">
                  <div
                    style="background-color: black;width: 29px;margin: 5px 0 5px 0;display: flex;flex-direction: column;justify-content: space-around;"
                  >
                    <span
                      style="font-family: PingFangSC-Regular;font-size: 10px;color: #FFFFFF;text-align: center;font-weight: 400;"
                    >#123</span>
                    <span
                      style="font-family: PingFangSC-Regular;font-size: 10px;color: #FFFFFF;text-align: center;font-weight: 400;"
                    >#123</span>
                  </div>
                  <div style="display: flex;flex-direction: column;margin: 5px 0 5px 0">
                    <div style="display: flex;flex-direction: row;">
                      <p
                        style="font-size: 12px;margin: 6px;line-height: 12px;color: #000;padding: 0 0 0 4px;display: flex;align-items: center;"
                        v-if="slotProps.data.treeNodeData.updateTime !== null && slotProps.data.treeNodeData.updateTime !== ''"
                      >{{ slotProps.data.treeNodeData.updateTime }}</p>
                    </div>
                    <div>
                      <p
                        style=" font-size: 12px;color: #4686f2; cursor: pointer;margin: 6px;"
                      >{{ slotProps.data.treeNodeData.instanceIp + ":" + slotProps.data.treeNodeData.instancePort }}</p>
                    </div>
                  </div>
                </div>
                <div class="child_wrapper_hidden">
                  <div style="padding: 4px 15px;display: flex;flex-direction: row;">
                    <div v-if="slotProps.data.treeNodeData.isShow">
                      <p
                        class="content_name"
                        v-if="slotProps.data.treeNodeData.agentStatus !== null && slotProps.data.treeNodeData.agentStatus !== ''"
                      >
                        agentStatus:{{
                          slotProps.data.treeNodeData.agentStatus
                        }}
                      </p>
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.instanceRole !== null &&
                          slotProps.data.treeNodeData.instanceRole !== ''
                        "
                      >
                        instanceRole:{{
                          slotProps.data.treeNodeData.instanceRole
                        }}
                      </p>
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.instanceStatus !== null &&
                          slotProps.data.treeNodeData.instanceStatus !== ''
                        "
                      >
                        instanceStatus:{{
                          slotProps.data.treeNodeData.instanceStatus
                        }}
                      </p>
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.repmgrdStatus !== null &&
                          slotProps.data.treeNodeData.repmgrdStatus !== ''
                        "
                      >
                        ltclusterdStatus:{{
                          slotProps.data.treeNodeData.repmgrdStatus
                        }}
                      </p>
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.keepalivedStatus !==
                          null &&
                          slotProps.data.treeNodeData.keepalivedStatus !== ''
                        "
                      >
                        keepalivedStatus:{{
                          slotProps.data.treeNodeData.keepalivedStatus
                        }}
                      </p>
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.syschronousState !==
                          null &&
                          slotProps.data.treeNodeData.syschronousState !== ''
                        "
                      >
                        syschronousState:{{
                          slotProps.data.treeNodeData.syschronousState
                        }}
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </template>
          </vue-tree>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import VueTree, { updatePartTree } from "../lib/vueTree.umd.min.js";
let mockData = [
  {
    id: null,
    hostId: 1,
    hostIdIncr: null,
    instanceId: 1884,
    instanceIdIncr: null,
    instanceIp: "192.168.10.128",
    instancePort: 5435,
    instancePortIncr: null,
    instanceRole: "",
    instanceStatus: "正常",
    repmgrdStatus: "running",
    keepalivedStatus: null,
    syschronousState: null,
    agentStatus: "正常",
    createTime: null,
    updateTime: "2022-02-17 10:25:43",
    upstreamId: null,
    upstreamIdIncr: null,
    virtualIp: null,
  },
  {
    id: null,
    hostId: null,
    hostIdIncr: null,
    instanceId: 1891,
    instanceIdIncr: null,
    instanceIp: "192.168.10.128",
    instancePort: 5433,
    instancePortIncr: null,
    instanceRole: "123",
    instanceStatus: "正常",
    repmgrdStatus: "running",
    keepalivedStatus: null,
    syschronousState: null,
    agentStatus: "故障",
    createTime: null,
    updateTime: "2022-02-17 10:25:43",
    upstreamId: 1884,
    upstreamIdIncr: null,
    virtualIp: null,
  },
  {
    id: null,
    hostId: null,
    hostIdIncr: null,
    instanceId: 1888,
    instanceIdIncr: null,
    instanceIp: "192.168.10.128",
    instancePort: 5434,
    instancePortIncr: null,
    instanceRole: "",
    instanceStatus: "正常",
    repmgrdStatus: "failed",
    keepalivedStatus: null,
    agentStatus: "未安装",
    syschronousState: "async",
    createTime: null,
    updateTime: "2022-02-17 10:25:43",
    upstreamId: 1884,
    upstreamIdIncr: null,
    virtualIp: null,
  },
];
export default {
  components: {
    "vue-tree": VueTree,
  },
  data() {
    return {
      rootData: [],
    };
  },
  methods: {
    al(slotProps) {
      slotProps.treeNodeData.isOpen = !slotProps.treeNodeData.isOpen;
      updatePartTree(slotProps.$treeNodeRefs.treeNodeRef);
    },
    getData() {
      this.rootData = this.filterPidTree(mockData)[0];
      console.log(this.rootData);
    },
    // 扁平结构转换成树结构
    filterPidTree(arrList) {
      const toTree = (id) => {
        let childList = [];
        let marchArr = arrList.filter((item) => {
          return item.upstreamId === id;
        });
        marchArr.forEach((item) => {
          item.children = toTree(item.instanceId);
          item.isOpen = true;
          item.isShow = true;
          childList.push(item);
        });
        return childList;
      };
      return toTree(null);
    },
  },
  mounted() { },
  created() {
    this.getData();
  },
  beforeDestroy() { },
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
    isOpen: false,
    children: [],
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
.area_wrapper {
  min-height: 372px;
  box-sizing: border-box;
  padding: 60px;
  overflow-x: scroll;
  border-radius: 4px;
  background-color: #f5f5f5;
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

#area_wrapper {
  min-height: 372px;
  box-sizing: border-box;
  padding: 60px;
  overflow: hidden;
  border-radius: 4px;
  position: absolute;
}

.parent_wrapper_hidden {
  box-shadow: 0px 14px 7px -4px #72a2f4, 0px -1px 6px -2px #72a2f4;
  width: 240px;
  padding: 0;
  box-sizing: border-box;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.15s ease;
  display: flex;
  flex-direction: column;
}
.child_wrapper_hidden {
  width: 240px;
  padding: 0;
  box-sizing: border-box;
  border-radius: 9px;
  cursor: pointer;
  overflow: hidden;
  background-color: #fff;
  transition: all 0.15s ease;
  display: flex;
  flex-direction: column;
}
</style>