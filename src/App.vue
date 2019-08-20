<template>
  <div id="app">
    <el-card>
      <div slot="header">KT-TREE-SELECT</div>
      <el-form>
        <el-form-item label="Tree Single：">
          <SelectTree
            v-model="value01"
            :data="data"
            :default-expanded-keys="defaultExpandedKeys"
            :filterable="true"
            :clearable="true"
          ></SelectTree>
          <div>{{value01}}</div>
          <el-button type="primary" size="mini" @click="clickMe01">Submit Single!</el-button>
        </el-form-item>
        <el-form-item label="Tree Multiple：">
          <SelectTree
            v-model="value02"
            :data="data"
            :default-expanded-keys="['12101']"
            :filterable="true"
            :multiple="true"
          ></SelectTree>
          <el-button type="primary" size="mini" @click="clickMe02">Submit Multiple!</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>


<script>
import SelectTree from "./components/treeSelect.vue";
export default {
  name: "app",
  components: {
    SelectTree
  },
  data () {
    return {
      data: [],
      defaultExpandedKeys: [],
      // value01: '12102',
      // value02: ['12103', '12104', '12105', '12106', '12107', '12108']
      value01: 'C1',
      value02: ['C1', 'C2', 'C3']

    }
  },
  created () {
    // setTimeout(() => {

    //   this.defaultExpandedKeys = this.data[0].children.map(item => item.id).concat(this.data[0].id);
    // }, 800);

    this.data = [
      {
        id: '中国',
        name: '中国',
        parentId: 'G0',
        children: [
          {
            id: 'S1', name: '广东', parentId: 'G0',
            children: [
              { id: 'C1', name: '广州', parentId: 'S1' },
              { id: 'C2', name: '深圳', parentId: 'S1' },
              { id: 'C3', name: '佛山', parentId: 'S1' }
            ]
          },
          { id: 'S2', name: '湖南', parentId: 'G0' },
          { id: 'S3', name: '广西', parentId: 'G0' },
          { id: 'S4', name: '湖北', parentId: 'G0' },
        ]
      }
    ];
  },
  methods: {
    clickMe01 () {
      alert(this.value01);
    },
    clickMe02 () {
      alert(this.value02.toString());
    }

  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/* 下拉选option */
.kt-select-tree__option {
  display: none;
  padding: 0 !important;
}

/* 弹出框 */
.kt-select-tree__popover {
  margin-top: 0 !important;
  padding: 5px 0;
}

/* 搜索框 */
.kt-select-tree__input {
  padding: 0 4px 4px !important;
}

/* tree树形控件 */
.kt-select-tree {
  overflow-y: scroll;
  max-height: 350px;
}

.kt-select-tree::-webkit-scrollbar {
  z-index: 11;
  width: 6px;
}

.kt-select-tree::-webkit-scrollbar-track,
.kt-select-tree::-webkit-scrollbar-corner {
  background: #fff;
}

.kt-select-tree::-webkit-scrollbar-thumb {
  width: 6px;
  border-radius: 5px;
  background: #b4bccc;
}

.kt-select-tree::-webkit-scrollbar-track-piece {
  width: 6px;
  background: #fff;
}

/* 覆盖默认高亮效果 */
.el-tree--highlight-current .el-tree-node.is-current > .el-tree-node__content {
  background-color: #409eff !important;
  color: #fff;
}
</style>