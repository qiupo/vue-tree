<template>
  <div class="bg-grey" style="height: 100%;background: #5b5b5b;">
    <div
      id="app"
      style="height: 100%; display: flex; flex-direction: column"
      v-cloak
    >
      <div class="pulse">
        <h-icon color="#5cb85c" name="success_fill"></h-icon>
        <div>
          <span class="text">节点拓扑图</span>
        </div>
        <div class="float-right">
          <div style="padding-right: 4px">
            <button type="ghost" shape="circle" @click="setBack">还原</button>
          </div>
        </div>
      </div>
      <!--        <div style="z-index: 99;background-color: #fff;padding: 16px 8px 8px 8px;">-->
      <!--            <h-tree @on-select-change="select" title-max-width="10" :data="baseData" :render="renderContent" s></h-tree>-->
      <!--        </div>-->
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
              <div
                class="parent_wrapper_hidden"
                :style="
                  slotProps.data.treeNodeData.agentStatus === '正常'
                    ? { backgroundColor: '#5cb85c' }
                    : { backgroundColor: '#ff0000' }
                "
              >
                <!--                         <div class="parent_content" @click.native="handlerNodeClick(slotProps.data)">-->
                <div style="display: flex; flex-direction: row">
                  <p
                    style="
                      font-size: 16px;
                      color: #fff;
                      padding: 0 0 0 15px;
                      display: flex;
                      align-items: center;
                    "
                  >
                    {{ "#" + slotProps.data.treeNodeData.instanceId }}
                  </p>
                  <p
                    style="
                      font-size: 12px;
                      line-height: 12px;
                      color: #fff;
                      padding: 0 0 0 4px;
                      display: flex;
                      align-items: center;
                    "
                    v-if="
                      slotProps.data.treeNodeData.updateTime !== null &&
                      slotProps.data.treeNodeData.updateTime !== ''
                    "
                  >
                    {{ slotProps.data.treeNodeData.updateTime }}
                  </p>
                  \
                  <button
                    class="float-right"
                    style="padding-top: 0; padding-bottom: 0; color: #fff"
                    type="text"
                    @click="
                      slotProps.data.treeNodeData.isShow
                        ? showPage(slotProps.data, false)
                        : showPage(slotProps.data, true)
                    "
                    :icon="
                      slotProps.data.treeNodeData.isShow
                        ? 'chevron-up'
                        : 'more1'
                    "
                  ></button>
                </div>
                <div class="child_wrapper_hidden">
                  <div
                    style="
                      display: flex;
                      flex-direction: row;
                      padding: 8px 0 0 0;
                    "
                  >
                    <button
                      style="padding: 0 0 0 15px"
                      type="text"
                      @click="
                        toTab(
                          slotProps.data.treeNodeData.instanceId,
                          slotProps.data.treeNodeData.agentStatus
                        )
                      "
                    >
                      <p
                        style="font-size: 16px; color: #4686f2; cursor: pointer"
                      >
                        <h-icon
                          color="#5cb85c"
                          style="font-size: 16px"
                          name="success_fill"
                          v-if="
                            slotProps.data.treeNodeData.agentStatus === '正常'
                          "
                        >
                        </h-icon>
                        <h-icon
                          color="#ff0000"
                          style="font-size: 14px"
                          name="error"
                          v-else
                        >
                        </h-icon
                        >{{
                          slotProps.data.treeNodeData.instanceIp +
                          ":" +
                          slotProps.data.treeNodeData.instancePort
                        }}
                      </p>
                    </button>
                  </div>
                  <div
                    style="
                      padding: 4px 15px;
                      display: flex;
                      flex-direction: row;
                    "
                  >
                    <div v-if="slotProps.data.treeNodeData.isShow">
                      <p
                        class="content_name"
                        v-if="
                          slotProps.data.treeNodeData.agentStatus !== null &&
                          slotProps.data.treeNodeData.agentStatus !== ''
                        "
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
                    <button
                      type="text"
                      v-if="slotProps.data.treeNodeData.children.length !== 0"
                      style="padding: 0"
                      class="float-right"
                      @click="al(slotProps.data)"
                      icon="chevron-right"
                    ></button>
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
let setBackTop = "0",
  setBackLeft = "0";
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
    instanceRole: "主节点",
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
    instanceId: 1891, //2
    instanceIdIncr: null,
    instanceIp: "192.168.10.128", //2
    instancePort: 5433, //2
    instancePortIncr: null,
    instanceRole: "witness节点", //2
    instanceStatus: "正常", //1
    repmgrdStatus: "running", //2
    keepalivedStatus: null, //2
    syschronousState: null, //2
    agentStatus: "故障", //1
    createTime: null,
    updateTime: null, //2
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
    instanceRole: "备节点",
    instanceStatus: "正常",
    repmgrdStatus: "failed",
    keepalivedStatus: null,
    agentStatus: "未安装",
    syschronousState: "async",
    createTime: null,
    updateTime: null,
    upstreamId: 1884,
    upstreamIdIncr: null,
    virtualIp: null,
  },
];
let nullData = [
  {
    id: null,
    isOpen: true,
    isShow: true,
    hostId: "暂无",
    instanceId: "暂无",
    instanceIp: "暂无",
    instancePort: "暂无",
    instanceRole: "暂无",
    instanceStatus: "暂无",
    repmgrdStatus: "暂无",
    keepalivedStatus: "暂无",
    syschronousState: "暂无",
    agentStatus: "暂无",
    updateTime: "暂无",
    upstreamId: null,
    virtualIp: null,
    children: [],
  },
];
export default {
  components: {
    "vue-tree": VueTree,
  },
  data() {
    return {
      rootData: [],
      dataBaseId: undefined,
      newName: "",
      activeNode: null,
      nameModalVisible: false,
      serverTime: new Date(),
      serverTimeInfo: true,
    };
  },
  methods: {
    handlerNodeClick(activeData) {
      console.log("handlerNodeClick", activeData);
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
      slotProps.treeNodeData.isOpen = !slotProps.treeNodeData.isOpen;
      updatePartTree(slotProps.$treeNodeRefs.treeNodeRef);
    },
    showPage(slotProps, val) {
      slotProps.treeNodeData.isShow = val;
      setTimeout(
        () => updatePartTree(slotProps.$treeNodeRefs.treeNodeRef),
        100
      );
    },
    // getHeight() {
    //     document.getElementById('area_wrapper').style.maxHeight= document.getElementById('app').offsetHeight-2+"px";
    // }
    getData() {
      // getPost(contextPath + '/db/getClusterInfoTopologicalGraph', {
      //     "data": {
      //         'instanceId': instanceId,
      //         'instanceType': instanceType
      //     }
      // }).then(res => {
      //     if (res.data !== null) {
      //         this.rootData = this.filterPidTree(res.data)[0];
      //     } else {
      //         this.rootData=nullData;
      //         // this.rootData = genTestData();
      //         this.$hMessage.error('getClusterInfoTopologicalGraph错误,' + '错误码:' + res.code + ';错误信息:' + res.msg);
      //     }
      // }).catch(err=>{
      //     this.rootData=nullData;
      //     this.$hMessage.error('getClusterInfoTopologicalGraph错误');
      // })
      // console.log(this.rootData,mockData)

      this.rootData = this.filterPidTree(mockData)[0];
      // this.rootData = genTestData();
      console.log(this.rootData);
    },
    toTab(id, status) {
      if (status === "正常") {
        window.top.uiTab.addTab(
          this.dataBaseId,
          "/em/tasystem/emPage/dataBase/dataBaseMain?menuflag=1&instanceId=" +
            id,
          "数据库概览"
        );
      } else {
        this.$hMessage.warning(
          "该节点" + status + "，agent故障,请检查agent的状态，错误码LEG15"
        );
      }
    },
    renderContent(h, { root, node, data }) {
      return h(
        "span",
        {
          style: {
            display: "inline-block",
            width: "100%",
          },
        },
        [
          h("span", [
            h("h-icon", {
              props: {
                name: "ios-paper-outline",
              },
              style: {
                marginRight: "8px",
              },
            }),
            h("span", data.title),
          ]),
          h("span", {
            style: {
              display: "inline-block",
              float: "right",
              marginRight: "32px",
            },
          }),
        ]
      );
    },
    append(data) {
      const children = data.children || [];
      children.push({
        title: "appended node",
        expand: true,
      });
      this.$set(data, "children", children);
    },
    remove(root, node, data) {
      console.log(root, node, data);
      const parentKey = root.find((el) => el === node).parent;
      const parent = root.find((el) => el.nodeKey === parentKey).node;
      const index = parent.children.indexOf(data);
      parent.children.splice(index, 1);
    },
    setBack() {
      let arm = document.getElementById("area_wrapper");
      console.log(arm, setBackTop);
      arm.style.top = setBackTop;
      arm.style.left = setBackLeft;
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
  mounted() {
    // this.getHeight();
    // window.addEventListener('resize', this.getHeight);
    let arm = document.getElementById("area_wrapper");
    setBackTop = arm.style.top;
    setBackLeft = arm.style.left;
  },
  created() {
    let storage = JSON.parse(localStorage.getItem("leftMenus"));
    storage &&
      storage
        .filter((item) => item.menuName === "数据库监控与管理")[0]
        .children.filter((item) => item.menuName === "数据库监控")[0]
        .children.forEach((item) => {
          item.menuName === "数据库概览" && (this.dataBaseId = item.id);
        });
    this.getData();
  },
  beforeDestroy() {
    // window.removeEventListener('resize', this.getHeight);
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
window.onload = function () {
  var oDiv = document.getElementById("tree"); // 获取到要拖拽的元素
  drag(oDiv); // 调用自己封装的拖拽函数

  function drag(obj) {
    let arm = document.getElementById("area_wrapper");
    obj.onmousedown = function (e) {
      var e = e || window.event; // 兼容 IE
      var divX = e.clientX;
      var divY = e.clientY;
      let y = arm.offsetTop;
      let x = arm.offsetLeft;
      if (obj.setCapture) {
        obj.setCapture(); // 修复低版本 IE bug
      }
      document.onmousemove = function (e) {
        obj.style.cursor = "pointer";
        var e = e || window.event;
        var disX = e.clientX - divX;
        var disY = e.clientY - divY;
        // 保存起始位置
        // 移动时重新得到物体的距离，解决拖动时出现晃动现象
        arm.style.top = y + disY + "px";
        arm.style.left = x + disX + "px";
        document.onmouseup = function () {
          // 鼠标抬起时不再移动
          // 预防鼠标弹起来后还会循环（即预防鼠标放上去的时候还会移动）
          document.onmousedown = null;
          document.onmousemove = null;
          if (obj.releaseCapture) {
            obj.releaseCapture(); // 修复低版本 IE bug
          }
        };
      };
      document.onmouseup = function () {
        // 鼠标抬起时不再移动
        // 预防鼠标弹起来后还会循环（即预防鼠标放上去的时候还会移动）
        document.onmousedown = null;
        document.onmousemove = null;
        if (obj.releaseCapture) {
          obj.releaseCapture(); // 修复低版本 IE bug
        }
      };
    };
  }
};
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
td {
  border: solid #add9c0;
  border-width: 0px 1px 1px 0px;
}
.text {
  margin: 0 8px;
  font-size: 16px;
  color: #333333;
  line-height: 16px;
  font-weight: 500;
}
.pulse {
  z-index: 99;
  background: #ffffff;
  display: flex;
  align-items: center;
  border-radius: 2px;
  padding: 8px 16px;
}
.float-right {
  float: right;
  margin-left: auto;
  display: flex;
  align-items: center;
}
table {
  border: solid #add9c0;
  border-width: 1px 0px 0px 1px;
}
.h-tabs .h-tabs-tabpane {
  background-color: #fff;
}

.h-tabs.h-tabs-card > .h-tabs-bar .h-tabs-tab,
.h-tabs.h-tabs-card-top > .h-tabs-bar-top .h-tabs-tab {
  background-color: #f0f0f0;
  border: 0px;
}

.h-tabs.h-tabs-card > .h-tabs-bar .h-tabs-tab-active {
  background-color: #fff;
  border: 1px solid;
}

.h-tabs-nav-container {
  background-color: #f0f0f0;
}

.h-tabs .h-tabs-bar {
  margin: 0;
}

#area_wrapper {
  min-height: 372px;
  box-sizing: border-box;
  padding: 60px;
  overflow: hidden;
  border-radius: 4px;
  position: absolute;
}

.parent_wrapper {
  display: inline-block;
  width: 400px;
  padding: 8px;
  box-sizing: border-box;
  /* border-left-width: 3px;
              border-left-style: solid;
              border-color: #F65656; */
  border-radius: 4px;
  cursor: pointer;
  overflow: hidden;
  background-color: #fff;
  transition: all 0.15s ease;
}
.parent_wrapper_hidden {
  /*display: inline-block;*/
  width: 240px;
  padding: 0;
  box-sizing: border-box;
  /* border-left-width: 3px;
              border-left-style: solid;
              border-color: #F65656; */
  border-radius: 9px;
  cursor: pointer;
  overflow: hidden;
  background-color: #5cb85c;
  transition: all 0.15s ease;
  display: flex;
  flex-direction: column;
}
.child_wrapper_hidden {
  width: 240px;
  padding: 0;
  box-sizing: border-box;
  /* border-left-width: 3px;
              border-left-style: solid;
              border-color: #F65656; */
  border-radius: 9px;
  cursor: pointer;
  overflow: hidden;
  background-color: #fff;
  transition: all 0.15s ease;
  display: flex;
  flex-direction: column;
}
.parent_content {
  display: flex;
  align-items: flex-start;
}

.content_name {
  flex: 1;
  line-height: 16px;
  padding-bottom: 4px;
  overflow: hidden;
  white-space: initial;
  word-break: break-all;
  font-size: 14px;
  color: #666;
  display: flex;
  align-items: center;
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
i {
  cursor: inherit;
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