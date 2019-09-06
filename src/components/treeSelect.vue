<template>
  <div>
    <el-popover
      ref="popover"
      popper-class="kt-select-tree__popover"
      placement="bottom-start"
      trigger="click"
    >
      <!-- 显示content -->
      <div>
        <el-input
          class="kt-select-tree__input"
          v-if="filterable"
          placeholder="请输入关键字"
          v-model="searchValue"
          size="mini"
        ></el-input>
        <el-tree
          ref="tree"
          class="kt-select-tree"
          :style="{'min-width':treeWidth}"
          :data="treeData"
          :props="dynaDefProps"
          :node-key="dynaDefProps.id"
          @node-click="onNodeClick"
          :highlight-current="!multiple"
          :expand-on-click-node="false"
          :default-expanded-keys="defaultExpandedKeys"
          :show-checkbox="multiple"
          :default-expand-all="defaultExpandAll"
          :filter-node-method="onTreeFilter"
          @check="onCheck"
          :load="loadNode"
          lazy
        >
          <span class="kt-select-tree_label" slot-scope="{ node, data }">
            <slot :node="node" :data="data">{{ node.label }}</slot>
          </span>
        </el-tree>
      </div>
      <!-- 触发html -->
      <el-select
        ref="select"
        slot="reference"
        popper-class="kt-select-tree__option"
        :style="{'width':width}"
        v-model="modelValue"
        :size="size"
        :multiple="multiple"
        :clearable="clearable"
        :collapse-tags="collapseTags"
        :placeholder="placeholder"
        @change="onChange"
        @clear="onClear"
      ></el-select>
    </el-popover>
  </div>
</template>

<script>
export default {
  name: "el-tree-select",
  model: {
    prop: 'value',
    event: 'input'
  },
  props: {

    data: Array,
    value: String | Array,
    defaultProps: Object,
    // 对外暴露的属性
    width: String,
    size: {
      type: String,
      default: 'mini'
    },
    placeholder: {
      type: String,
      default: '请选择'
    },
    multiple: {
      type: Boolean,
      default: false
    },
    filterable: {
      type: Boolean,
      default: false
    },
    clearable: {
      type: Boolean,
      default: false
    },
    defaultExpandedKeys: Array,
    defaultExpandAll: {
      type: Boolean,
      default: false
    },
    collapseTags: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      // 传参的值
      modelValue: void 0,
      // 展示框的值
      labelValue: void 0,
      // 搜索框的值
      searchValue: void 0,
      // 树节点宽度
      treeWidth: 'auto',
      // 扁平化数据
      options: []

    }
  },
  computed: {
    // 树节点配置选项
    dynaDefProps () {
      return Object.assign({}, {
        parentId: 'parentId',
        id: 'id',
        label: 'name',
        children: 'children',
        filter: 'filter'
      }, this.defaultProps)
    },
    // 树节点数据
    treeData () {

      if (this.data.length > 0) {
        // 备份降维模型
        // this.options = this.flattenTree(this.data);
        // ⭐⭐⭐ 初始化绑定节点 ⭐⭐⭐
        this.$nextTick(() => {
          // ⭐⭐⭐ value -> modelValue
          if (this.multiple) {
            if (Array.isArray(this.value) && this.value.length > 0) {
              this.labelValue = this.options.filter(node => this.value.includes(node[this.dynaDefProps.id])).map(node => node[this.dynaDefProps.label]);
              console.log('this.labelVa999lue', this.labelValue)
              console.log('this.labelValue', this.data)
              this.$refs.tree.setCheckedKeys(this.value);
              this.modelValue = this.value;
            }
          } else {
            if (this.value) {
              this.labelValue = this.options.filter(node => node[this.dynaDefProps.id] === this.value).map(node => node[this.dynaDefProps.label])[0];
              this.$refs.tree.setCurrentKey(this.value);

              this.modelValue = this.value;
            }
          }
        });
        // 初始化树模型
        return this.data;
      }
    }
  },
  created () {
    // 获取输入框宽度同步至树状菜单宽度
    this.$nextTick(() => {
      this.treeWidth = `${(this.width || this.$refs.select.$refs.reference.$el.clientWidth) - 24}px`;
    });
  },

  mounted () {
    this.valueId = this.value,    // 初始值
      console.log('this.value', this.value)
    // this.initHandle()
  },
  methods: {
    loadNode (node, resolve) {
      if (node.level === 0) {
        return resolve([{ id: '中国1', name: 'kkegion' }, { id: '中国2', name: 'region' }]);
      }
      if (node.level > 1) return resolve([]);

      setTimeout(() => {
        const data = [{
          id: '中国3',
          name: 'leaf',
          leaf: true
        }, {
          id: '中国4',
          name: 'zone'
        }];

        resolve(data);
      }, 500);
    },
     loadNode(node, resolve) {
      //结构树接口
      if (node.level === 0) {
        var params = {
          isPortal: true,
          dimensionType: 'BUBI_RND_TEAM',
          relativeLevel: 1,
          parentCode: -1,
          // MTE_TO_TENANT_CODE: 'BUOA'
        }
        ajax.get(this.treeURl, { params }).then(res => {
          return resolve(res.data.result);
        })

      }
      if (node.level >= 1) {
        var params = {
          isPortal: true,
          dimensionType: 'BUBI_RND_TEAM',
          relativeLevel: 1,
          parentCode: node.data.code,
          // MTE_TO_TENANT_CODE: 'BUOA'
        }
        console.log('node', node.data.code)
        ajax.get(this.treeURl, { params }).then(res => {
          console.log('this.valueresultresult', res.data.result)
          return resolve(res.data.result);
        })
      }
    },
    /* 点击节点 */
    onNodeClick (node, data) {
      if (!this.multiple) {
        // 绑定值
        this.modelValue = node[this.dynaDefProps.id];
          this.modelValue=node.name
        // 对外暴露绑定函数
        this.$emit('node-click', node, data);
        // 隐藏菜单
        console.log(' this.modelValue', this.modelValue)
        this.onCloseTree();
      }
    },
    /* 点击节点复选框 */
    onCheck (node, data) {
      if (this.multiple) {
        let juniorNodes = data.checkedNodes.filter(node => !node[this.dynaDefProps.children]);
        // 绑定值
        this.modelValue = juniorNodes.map(node => node[this.dynaDefProps.id]);
        // 对外暴露绑定函数
        this.$emit('check', node, data);
      }
    },
    /* 节点过滤函数 */
    onTreeFilter (value, data) {
      if (!value) return true;
      return data[this.dynaDefProps.filter] ? data[this.dynaDefProps.label].indexOf(value) !== -1 || data[this.dynaDefProps.filter].indexOf(value.toUpperCase()) !== -1 : data[this.dynaDefProps.label].indexOf(value) !== -1;
    },
    /* 延迟过滤节点 */

    /* popover菜单关闭 */
    onCloseTree () {
      this.$refs.popover.showPopper = false;
    },
    /* 下拉选value变化 */
    onChange (val) {
      if (this.multiple) {
        this.labelValue = val;
        this.modelValue = this.options.filter(node => val.includes(node[this.dynaDefProps.label])).map(node => node[this.dynaDefProps.id]);
        this.$nextTick(() => {
          this.$refs.tree.setCheckedKeys(this.modelValue);
        })
      }
    },
    /* 单选模式下清空 */
    onClear () {
      this.modelValue = void 0;
    },
    /* 数组降维 */
    // flattenTree (arr = []) {
    //   let result = [], copy = $.extend(true, [], arr); // 深度拷贝数组
    //   const fun = (arr) => {
    //     arr.forEach(node => {
    //       result.push(node);
    //       if (this.isArray(node)) fun(node[this.dynaDefProps.children]);
    //       delete node[this.dynaDefProps.children];
    //     });
    //   };
    //   fun(copy);
    //   return result;
    // },
    /* 判断数组 */
    isArray (data) {
      return data[this.dynaDefProps.children] && Array.isArray(data[this.dynaDefProps.children]) && data[this.dynaDefProps.children].length > 0
    }



  },

  watch: {
    searchValue (val) {
      // this.debounceQuery(val);
      this.$refs.tree.filter(val);
    },
    modelValue (val) {
      // ⭐⭐⭐ modelValue -> labelValue
      if (val && Array.isArray(this.options) && this.options.length > 0) {
        if (this.multiple) this.labelValue = this.options.filter(node => val.includes(node[this.dynaDefProps.id])).map(node => node[this.dynaDefProps.label]);
        else this.labelValue = this.options.filter(node => node[this.dynaDefProps.id] === val)[0][this.dynaDefProps.label];
      }
      // ⭐⭐⭐ modelValue -> value
      this.$emit('input', val);
    },
    // ⭐⭐⭐ value -> labelValue
    value (val) {
      if (!val) this.labelValue = void 0;
    }
  },
}
</script>

<style scoped>
.el-scrollbar .el-scrollbar__view .el-select-dropdown__item {
  height: auto;
  max-height: 274px;
  padding: 0;
  overflow: hidden;
  overflow-y: auto;
}
.el-select-dropdown__item.selected {
  font-weight: normal;
}
ul li >>> .el-tree .el-tree-node__content {
  height: auto;
  padding: 0 20px;
}
.el-tree-node__label {
  font-weight: normal;
}
.el-tree >>> .is-current .el-tree-node__label {
  color: #409eff;
  font-weight: 700;
}
.el-tree >>> .is-current .el-tree-node__children .el-tree-node__label {
  color: #606266;
  font-weight: normal;
}
</style>
