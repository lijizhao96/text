<template>
  <div class="screen">
    <ul class="one-event">
      <li class="one-event-item" v-for="(item, index) in list" :key="index">
        <div
          style="display: flex; align-items: center; justify-content: space-between; padding: 10px 12px;"
        >
          <div>
            <span class="line-prefix">{{indexList[index]}}</span>
            <el-select v-model="item.name" placeholder="请选择" size="small" style="width:120px;">
              <el-option-group v-for="group in optionsOne" :key="group.label" :label="group.label">
                <el-option
                  v-for="item in group.options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-option-group>
            </el-select>
            <span style="margin: 0 8px;">的</span>
            <el-cascader
              :show-all-levels="false"
              size="small"
              style="width:100px;"
              v-model="item.result"
              :options="optionsTwo"
              :props="{ expandTrigger: 'hover' }"
            ></el-cascader>
            <i class="el-icon-delete del" v-if="list.length >= 2" @click="delConditions(index)"></i>
          </div>
          <div>
            <el-button icon="el-icon-plus" type="text" @click="addScreen(index)">筛选条件</el-button>
          </div>
        </div>

        <ul v-if="item.screenList.length >= 1" class="screenList">
          <div class="line" :style="{height: item.screenList.length * 45 + 'px'}"></div>
          <div class="and">
            <el-button
              circle
              v-if="item.screenList.length >= 2"
              @click="andIs(index)"
            >{{item.isAnd ? '且' : '或'}}</el-button>
          </div>
          <div class="screenList-box">
            <li v-for="(items, indexs) in item.screenList" :key="indexs" class="screenList-item">
              <el-select
                v-model="items.condition"
                placeholder="请选择"
                size="small"
                style="width:200px;"
              >
                <el-option-group
                  v-for="group in optionsOne"
                  :key="group.label"
                  :label="group.label"
                >
                  <el-option
                    v-for="item in group.options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  ></el-option>
                </el-option-group>
              </el-select>

              <el-select
                v-model="item.middle"
                placeholder="请选择"
                size="small"
                style="width:90px;margin-left: 20px;"
              >
                <el-option
                  v-for="item in optionsTab"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
              <el-input
                v-model="items.result"
                placeholder="请输入内容"
                size="small"
                style="width: 200px;margin-left: 20px"
              ></el-input>
              <i class="el-icon-delete del" @click="delSecConditions(index, indexs)"></i>
            </li>
          </div>
        </ul>
      </li>
    </ul>

    <ul class="screen-bottom">
      <i class="el-icon-circle-plus plus" @click="addList"></i>
      <li class="screen-bottom-item" v-for="(item, index) in bottomList" :key="index">
        <span>按</span>
        <el-select
          v-model="item.name"
          placeholder="请选择"
          size="small"
          style="width:200px;margin: 0 20px;"
        >
          <el-option-group v-for="group in optionsOne" :key="group.label" :label="group.label">
            <el-option
              v-for="item in group.options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-option-group>
        </el-select>
        <span>查看</span>
        <div class="del-box">
          <i class="el-icon-delete del" v-if="bottomList.length >= 2" @click="delBottom(index)"></i>
        </div>
        <i class="el-icon-plus add" v-if="bottomList.length === index + 1" @click="addBottom"></i>
      </li>
    </ul>

    <ul class="screenList">
      <div class="line" :style="{height: bottomSecList.length * 45 + 'px'}"></div>
      <div class="and">
        <el-button
          circle
          v-if="bottomSecList.length >= 2"
          @click="secIsAnd = !secIsAnd"
        >{{secIsAnd ? '且' : '或'}}</el-button>
      </div>
      <div class="screenList-box">
        <li v-for="(items, indexs) in bottomSecList" :key="indexs" class="screenList-item">
          <el-select v-model="items.condition" placeholder="请选择" size="small" style="width:200px;">
            <el-option-group v-for="group in optionsOne" :key="group.label" :label="group.label">
              <el-option
                v-for="item in group.options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-option-group>
          </el-select>

          <el-select
            v-model="items.middle"
            placeholder="请选择"
            size="small"
            style="width:90px;margin-left: 20px;"
          >
            <el-option
              v-for="item in optionsTab"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>

          <el-input
            v-model="items.result"
            placeholder="请输入内容"
            size="small"
            style="width: 200px;margin-left: 20px"
          ></el-input>
          <i class="el-icon-delete del"></i>
        </li>
      </div>
    </ul>
    <div class="add-bottom-sec">
      <el-button icon="el-icon-plus" type="text" @click="addBottomSec">筛选条件</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Screen",
  props: {
    msg: String
  },
  data() {
    return {
      indexList: [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z"
      ],

      // 主要条件列表
      list: [
        {
          name: "",
          result: "",
          isAnd: false,
          screenList: []
        }
      ],

      // 第一个选项数据
      optionsOne: [
        {
          label: "默认分组",
          options: [
            {
              value: "pvp",
              label: "PVP战斗"
            },
            {
              value: "jq",
              label: "剧情"
            }
          ]
        },
        {
          label: "全部",
          options: [
            {
              value: "pvp",
              label: "PVP战斗"
            },
            {
              value: "jq",
              label: "剧情"
            },
            {
              value: "fb",
              label: "副本"
            },
            {
              value: "nd",
              label: "难度"
            }
          ]
        }
      ],

      // 第二个选项数据
      optionsTwo: [
        {
          value: "px",
          label: "屏幕宽度",
          children: [
            {
              value: "max",
              label: "最大值"
            },
            {
              value: "min",
              label: "最小值"
            }
          ]
        },
        {
          value: "num",
          label: "用户数"
        },
        {
          value: "zo",
          label: "总次数"
        }
      ],

      optionsTab: [
        {
          value: "1",
          label: "大于"
        },
        {
          value: "0",
          label: "等于"
        },
        {
          value: "2",
          label: "小于"
        }
      ],

      // 下面的筛选条件
      bottomList: [
        {
          name: "",
          screenList: []
        }
      ],

      // 下面的次级筛选
      bottomSecList: [],

      secIsAnd: false
    };
  },

  methods: {
    // 添加次级条件
    addScreen(index) {
      this.list[index].screenList.push({
        condition: "",
        middle: "",
        result: ""
      });
    },

    // 且或切换
    andIs(index) {
      this.list[index].isAnd = !this.list[index].isAnd;
    },

    // 删除次级条件
    delSecConditions(index, indexs) {
      this.list[index].screenList.splice(indexs, 1);
    },

    // 添加主要条件
    addList() {
      if (this.list.length === 26) {
        return;
      }
      this.list.push({
        name: "",
        result: "",
        isAnd: false,
        screenList: []
      });
    },

    // 删除主要条件
    delConditions(index) {
      this.list.splice(index, 1);
    },

    // 添加下级筛选
    addBottom() {
      this.bottomList.push({
        name: "",
        screenList: []
      });
    },

    // 删除下级筛选
    delBottom(index) {
      this.bottomList.splice(index, 1);
    },

    // 添加下级次级筛选
    addBottomSec() {
      this.bottomSecList.push({
        isAnd: false
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.screen {
  .one-event {
    .one-event-item {
      box-sizing: border-box;
      // padding: 10px 12px;

      .line-prefix {
        font-size: 14px;
        background: #c3e6d0;
        color: #2dca93;
        display: inline-block;
        width: 20px;
        border-radius: 2px;
        text-align: center;
        margin: 0 10px;
      }
    }
    .one-event-item:hover {
      background-color: #e0ecfd;
      transition: 0.5s;
      .del {
        display: inline-block;
      }
    }
    .del {
      display: none;
    }
  }
}

.screenList {
  display: flex;
  align-items: center;
  .line {
    width: 2px;
    background-color: #6fd3b3;
    height: 100%;
    margin: 0 20px 0 50px;
  }
  .and {
    width: 40px;
    margin-right: 20px;
  }
  .del {
    display: none;
  }
  .screenList-box {
    width: 100%;
    .screenList-item {
      height: 50px;
      display: flex;
      align-items: center;
    }
  }
}

.screenList:hover {
  background: #c3e6d0;
  transition: 0.5s;
  .line-prefix {
    transition: 0.5s;
    background: #c3e6d0ce;
  }
  .del {
    display: inline-block;
  }
}

.del {
  margin-left: 10px;
  color: #666;
  cursor: pointer;
}
.del:hover {
  color: #f56c6c;
}

.screen-bottom {
  // border-bottom: 1px solid #d9dadb;
  border-top: 1px solid #d9dadb;
  position: relative;

  .plus {
    position: absolute;
    top: -7px;
    left: 15px;
    cursor: pointer;
  }
  .screen-bottom-item {
    height: 60px;
    display: flex;
    align-items: center;
    padding-left: 50px;
    .add {
      display: inline-block;
      margin: 0 10px;
      cursor: pointer;
    }
    .del-box {
      display: none;
    }
  }
  .screen-bottom-item:hover {
    background: #c3e6d0;
    transition: 0.5s;
    .del-box {
      display: inline-block;
    }
  }
}
.screen-bottom:hover {
  background: #e0ecfd;
  transition: 0.5s;
}

.add-bottom-sec {
  height: 50px;
  border-bottom: 1px solid #d9dadb;
  display: flex;
  align-items: center;
  padding-left: 30px;
}
</style>
