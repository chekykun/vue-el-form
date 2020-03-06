<template>
  <div class="log-row">
    <span v-if="collapse">
      <el-collapse v-model="activeNames" @change="handleChange">
        <el-collapse-item name="1">
          <el-row class="row-tip" slot="title" style>
            <label class="log-title">{{title}}</label>
            <div class="icon-right icon-shr"></div>
          </el-row>
          <div style="padding:0 15px;">
            <slot></slot>
          </div>
        </el-collapse-item>
      </el-collapse>
    </span>
    <span v-else>
      <el-row class="row-tip" style>
        <label class="log-title">{{title}}</label>
      </el-row>
      <div style="padding:0 15px;">
        <slot></slot>
      </div>
    </span>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: ""
    },
    collapse: {
      default: false
    },
    value: {
      default: true
    }
  },
  created() {
    this.changeShow(this.value);
  },
  watch: {
    value(val) {
      this.changeShow(val);
    },
    activeNames(val) {
      this.$emit("input", val.length == 1);
    }
  },
  data() {
    return {
      activeNames: ["1"]
    };
  },
  methods: {
    handleChange(e) {},
    changeShow(flag) {
      if (flag) {
        _.remove(this.activeNames);
        this.activeNames.push("1");
      } else {
        _.remove(this.activeNames);
      }
    }
  }
};
</script>

<style <style lang="scss">
.log-row {
  margin-bottom: 0.5rem;
  .el-collapse-item__header {
    border-bottom: none;
  }
  .el-collapse-item__arrow {
    line-height: 34px;
  }
  .el-collapse {
    border-top: none;
  }
  .icon-right {
    float: right;
    width: 23px;
    height: 23px;
    background-image: url("./lib/icon_up.png");
    background-size: contain;
    opacity: 0.7;
    margin-right: -7px;
    margin-top: 4px;
    transition: transform 0.5s ease;
  }
  .is-active .icon-right {
    transform: rotateX(180deg);
    transition: all 0.5s ease;
  }
}
.row-tip {
  margin-bottom: 1rem;
  text-align: left;
  border-bottom: 1px solid #dcdfe6;
  height: 2rem;
  line-height: 2rem;
  background: linear-gradient(to bottom, #f4fafd, #9de2f1, #d6f3fd);
}
.log-title {
  padding: 0 10px;
  font-size: 1rem;
  border-bottom: 2px solid #61d4ff;
  height: 100%;
  display: inline-block;
}
</style>


