<template>
  <div class="lyout">
    <div class="main">
      <div class="leftsideconrent" ref="leftside">
        <div class="leftside">
          <div style="background:black;opacity: 0.9;" class="menu">
            <!-- :default-active="'/'+this.$store.state.data.subindex" -->
            <el-menu
              :collapse-transition="false"
              :unique-opened="true"
              text-color="gray"
              background-color="rgb(44, 42, 42)"
              active-text-color="white"
              class="el-menu-vertical-demo"
              :collapse="isCollapse"
              @select="select"
            >
              <el-submenu
                :index="'/'+item.parentindex"
                v-for="(item) in menu"
                :key="item.parentindex"
              >
                <template slot="title">
                  <div @mouseover="mouseover(item)" @mouseout="mouseout(item)" @click="click(item)">
                    <span slot="title">{{item.project}}</span>
                  </div>
                </template>
                <el-menu-item-group style="text-indent:15px;">
                  <div
                    v-for="item in item.submenu"
                    :key="item.menu"
                    @mouseover="submouseover(item)"
                  >
                    <el-menu-item :index="'/'+item.subindex">{{item.menu}}</el-menu-item>
                  </div>
                </el-menu-item-group>
              </el-submenu>
            </el-menu>
          </div>
        </div>
      </div>
      <div class="rightside" ref="rightside">
        <!-- <router-view></router-view> -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'lyout',
  data () {
    return {
      isCollapse: false,
      layershow: false,
      num: 1,
      menu: [
        {
          parentindex: 1,
          imgshow: true,
          imgshow0: false,
          imgshow2: false,
          // 默认时的图片

          project: '项目数据',
          submenu: [
            {
              subindex: 11,
              menu: '数据池',
              path: 'dataPool',
              index: 0
            },
            {
              subindex: 12,
              menu: '产品定制',
              path: 'productCustom',
              index: 0
            }
          ]
        },
        {
          parentindex: 2,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '数据模型',
          submenu: [
            {
              subindex: 21,
              menu: '数据模型数据池',
              path: 'dataPool',
              index: 1
            },
            {
              subindex: 22,
              menu: '数据模型产品定制',
              path: 'productCustom',
              index: 1
            }
          ]
        },
        {
          parentindex: 3,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '构件数据',
          submenu: [
            {
              subindex: 31,
              menu: '构件数据数据池',
              path: 'dataPool',
              index: 2
            },
            {
              subindex: 32,
              menu: '构件数据产品定制',
              path: 'productCustom',
              index: 2
            }
          ]
        },
        {
          parentindex: 4,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '业务数据',
          submenu: [
            {
              subindex: 41,
              menu: '数据模型数据池',
              path: 'dataPool',
              index: 3
            },
            {
              subindex: 42,
              menu: '数据模型产品定制',
              path: 'productCustom',
              index: 3
            }
          ]
        },
        {
          parentindex: 5,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '数据管理',
          submenu: [
            {
              subindex: 51,
              menu: '数据源管理',
              path: 'Processnodepool',
              index: 4
            },
            {
              subindex: 52,
              menu: '模型数据管理',
              path: 'Standardcomponentdata',
              index: 4
            },
            {
              subindex: 53,
              menu: '数据关系规则管理',
              path: 'Originalitymodelrelation',
              index: 4
            },
            {
              subindex: 54,
              menu: '数据关系应用管理',
              path: 'Projectrelationship',
              index: 4
            },
          ]
        },
        {
          parentindex: 6,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '产品管理',
          submenu: [
            {
              subindex: 61,
              menu: '产品信息',
              path: 'dataPool',
              index: 5
            },
            {
              subindex: 62,
              menu: '数据模型产品定制',
              path: 'productCustom',
              index: 5
            }
          ]
        },
        {
          parentindex: 7,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '权限管理',
          submenu: [
            {
              subindex: 71,
              menu: '平台侧角色管理',
              path: 'platformSide',
              index: 6
            },
            {
              subindex: 72,
              menu: '产品侧角色管理',
              path: 'productSide',
              index: 6
            }
          ]
        },
        {
          parentindex: 8,
          imgshow: false,
          imgshow0: false,
          imgshow2: true,

          project: '报表中心',
          submenu: [
            {
              subindex: 81,
              menu: '数据模型数据池',
              path: 'dataPool',
              index: 7
            },
            {
              subindex: 82,
              menu: '数据模型产品定制',
              path: 'productCustom',
              index: 7
            }
          ]
        }
      ]
    }
  },
  created () { },
  mounted () {
    // console.log('/' + this.$store.state.data.subindex)
  },
  methods: {
    mouseover (num) {
      // 选中时的图片
      this.menu[num.parentindex - 1].imgshow = false
      // 经过时的图片
      this.menu[num.parentindex - 1].imgshow0 = true
      // 默认时的图片
      this.menu[num.parentindex - 1].imgshow2 = false
    },
    mouseout (num) {
      this.menu[num.parentindex - 1].imgshow = false
      this.menu[num.parentindex - 1].imgshow0 = false
      this.menu[num.parentindex - 1].imgshow2 = true
    },
    click (num) {
      for (let i = 0; i < this.menu.length; i++) {
        this.menu[i].imgshow2 = true
        this.menu[i].imgshow = false
      }
      this.menu[num.parentindex - 1].imgshow = true
      this.menu[num.parentindex - 1].imgshow0 = false
      this.menu[num.parentindex - 1].imgshow2 = false
    },
    submouseover (num) {
      this.menu[num.index].imgshow = true
      this.menu[num.index].imgshow0 = false
      this.menu[num.index].imgshow2 = false
    },
    Packup () {
      if (this.isCollapse === false) {
        this.isCollapse = !this.isCollapse
        this.$nextTick(function () {
          this.$refs.leftside.style.width = '70px'
          this.$refs.rightside.style.width = 'calc(100% - 70px)'
        })
      } else {
        this.isCollapse = !this.isCollapse
        this.$refs.leftside.style.width = '200px'
        this.$refs.rightside.style.width = 'calc(100% - 200px)'
      }
    },
    gotohome () {
      this.$router.push({ name: 'home' })
      this.$store.commit('savedata', { subindex: 11 })
    },
    select (index, indexPath) {
      console.log(index.substr(1), indexPath)
      this.$store.commit('savedata', { subindex: index.substr(1) })
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* @import './layout.css'; */
.lyout {
  width: 100%;
  height: 100%;
}
.header {
  width: 100%;
  height: 50px;
  display: flex;
}
.logo {
  background: #181a1d;
  height: 50px;
  width: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.header_right {
  display: flex;
  justify-content: space-between;
  width: calc(100% - 200px);
  align-items: center;
}
.iscollapse {
  width: 20px;
  height: 22px;
  margin: 14px 15px;
}

.main {
  width: 100%;
  height: calc(100% - 50px);
  display: flex;
  position: relative;
}
.leftsideconrent {
  width: 200px;
  height: 100%;
  background-color: rgb(63, 60, 60);
  position: relative;
}
.leftside {
  width: 100%;
  height: 100%;
}
.rightside {
  width: calc(100% - 200px);
  height: 100%;
}
/* -------------------------------------------------------------- 侧边导航的样式*/
.menu .el-menu {
  border: 0px;
}
.menu .el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  height: 100%;
  border: 0px;
  background: #1890ff;
}
.el-menu-item:focus,
.el-menu-item:hover {
  background-color: #474d5d !important;
  opacity: 1;
}
.el-menu-item.is-active {
  font-weight: bold;
}
.el-menu-item,
.el-submenu__title {
  height: 44px;
  line-height: 44px;
  position: relative;
  -webkit-box-sizing: border-box;
  white-space: nowrap;
  list-style: none;
}

/* -------------------------------------------------------------- 侧边导航的样式*/

.layer {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  background-color: #242730;
  opacity: 0.3;
  z-index: 50;
}
.photo {
  width: 32px;
  height: 32px;
  margin: 10px 11px 12px 0px;
  border-radius: 50%;
}
.submenuimg {
  width: 24px;
  height: 24px;
  margin-right: 15px;
}
</style>
