<template>
  <div>
    <el-select
      v-model="value"
      placeholder="请选择"
      filterable
      multiple
      v-el-select-loadmore="loadmore"
    >
      <el-option v-for="item in options" :key="item.id" :label="item.label" :value="item.id"></el-option>
    </el-select>
  </div>
</template>
<script>

export default {
  directives: {
    'el-select-loadmore': {
      bind (el, binding) {
        // 获取element-ui定义好的scroll盒子
        const SELECTWRAP_DOM = el.querySelector('.el-select-dropdown .el-select-dropdown__wrap');
        SELECTWRAP_DOM.addEventListener('scroll', function () {
          /**
          * scrollHeight 获取元素内容高度(只读)
          * scrollTop 获取或者设置元素的偏移值,常用于, 计算滚动条的位置, 当一个元素的容器没有产生垂直方向的滚动条, 那它的scrollTop的值默认为0.
          * clientHeight 读取元素的可见高度(只读)
          * 如果元素滚动到底, 下面等式返回true, 没有则返回false:
          * ele.scrollHeight - ele.scrollTop === ele.clientHeight;
          */
          const condition = this.scrollHeight - this.scrollTop <= this.clientHeight;
          if (condition) {
            binding.value();
          }
        });
      }
    }
  },
  data () {
    return {
      value: '',
      options: [{
        value: '选项1',
        label: '黄金糕'
      }, {
        value: '选项2',
        label: '双皮奶'
      }, {
        value: '选项3',
        label: '蚵仔煎'
      }, {
        value: '选项4',
        label: '龙须面'
      }, {
        value: '选项5',
        label: '北京烤鸭'
      }],
      formData: {
        pageIndex: 1,
        pageSize: 20,
      }
    };
  },
  mounted () {
    this.getList(this.formData);
  },
  methods: {
    loadmore () {
      this.formData.pageIndex++;
      // this.getList(this.formData);
      console.log('this.formData.pageIndex', this.formData.pageIndex)
      if (this.formData.pageIndex === 2) {
        this.options = this.options.concat([
          {
            value: '选项4',
            label: '龙须面'
          }, {
            value: '选项5',
            label: '北京烤鸭'
          }
        ])
      } else if (this.formData.pageIndex === 3) {
        this.options = this.options.concat([
          {
            value: '选项4',
            label: '龙须gfdg面'
          }, {
            value: '选项5',
            label: '北京bfgf烤鸭'
          }
        ])
      }

    },
    getList (formData) {
      // 这里是接口请求数据, 带分页条件
      const _res = [1, 2, 3]; // 请求得到的数据
      this.options = [...this.options, ..._res];
    }
  }
};

</script>
