<template>
  <el-container>
    <el-main>
      <div class="tab">
        <!-- 头部 -->
        <i
          style="cursor: pointer;"
          :class="isShowTab ? 'el-icon-folder-remove' : 'el-icon-folder-add'"
          @click="showTab"
        ></i>
        <span class="tab-title">请添加指标</span>
        <span class="division"></span>
        <el-dropdown trigger="click">
          <span class="el-dropdown-link">
            事件
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>事件</el-dropdown-item>
            <el-dropdown-item>Session</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
      <!-- 筛选 -->
      <el-collapse-transition>
        <Screen v-show="isShowTab"></Screen>
      </el-collapse-transition>

      <!-- 图表 -->
      <div class="chart-top">
        <el-date-picker
          style="width:230px;"
          v-model="date"
          type="daterange"
          align="right"
          size="small"
          unlink-panels
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :picker-options="pickerOptions"
        ></el-date-picker>
      </div>
      <G2Line :height="380"></G2Line>
    </el-main>
  </el-container>
</template>

<script>
import Screen from "@/components/screen";
import G2Line from "@/components/line";

export default {
  name: "app",
  components: {
    Screen,
    G2Line
  },

  data: () => {
    return {
      isShowTab: true,
      date: null,
      pickerOptions: {
        shortcuts: [
          {
            text: "最近一周",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近一个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近三个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit("pick", [start, end]);
            }
          }
        ]
      }
    };
  },

  methods: {
    // 顶部tab点击事件
    showTab() {
      this.isShowTab = !this.isShowTab;
    }
  }
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  font-size: 14px;
  color: #4b5764;
}
.el-container {
  padding: 30px;
  background: #f7f9fa;
  .el-main {
    background: #fff;
    padding: 0;
    overflow: hidden;

    .tab {
      padding-left: 15px;
      height: 30px;
      display: flex;
      align-items: center;
      border-bottom: 1px solid #d9dadb;
      overflow: hidden;
      .tab-title {
        display: inline-block;
        margin-left: 10px;
      }
      .division {
        display: inline-block;
        width: 1px;
        height: 15px;
        background-color: #d9dadb;
        margin: 0 10px;
      }
      .el-dropdown-link {
        cursor: pointer;
      }
    }

    .chart-top {
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 70px;
      padding: 0 20px;
    }
  }
}
</style>
