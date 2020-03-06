<template>
  <div class="vue-form" :class="config['btns-position']" vm>
    <el-form
      :class="'form-body '+(layout[0][0].type!='tip'?'form-notip':'') "
      :rules="config['rules']"
      :label-position="config['label-position']||'right'"
      :label-width="config['label-width']||'120px'"
      :label-suffix="config['label-suffix']"
      :show-message="config['show-message']"
      :inline-message="config['inline-message']"
      :status-icon="config['status-icon']"
      :size="config['size']"
      v-loading="loading"
      :disabled="config['disabled']"
      ref="form"
      :model="values"
    >
      <!-- 表单项 -->
      <div v-for="(row,i) in layout" :key="i">
        <!-- 标题 -->
        <tip-title
          v-if="row.length==1&&row[0].type=='tip'"
          :title="row[0].title"
          :collapse="row[0].collapse"
          v-model="config.active"
        >
          <span v-for="(r,ind) in layout" :key="ind">
            <span v-if="r[0].rowIndex!=-1&&r[0].rowIndex==row[0].rowIndex&&r[0].type!='tip'">
              <vue-el-table
                v-if="r.length==1&&r[0].type=='table'"
                :configs="r[0].configs"
                :values="defaultValues.values"
              ></vue-el-table>
              <el-row v-else>
                <el-col v-for="(field, j) in r" :key="j" :span="span(field)">
                  <el-row v-if="field.type=='multi'">
                    <el-col v-for="(cfield,k) in field.childs" :key="k" :span="span(cfield)">
                      <el-row v-if="cfield.type=='button'">
                        <el-col>
                          <button-field :options="cfield"></button-field>
                        </el-col>
                      </el-row>
                      <slot v-else-if="cfield.type=='slot'" :name="cfield.slot"></slot>
                      <form-item-field :options="cfield" v-else-if="cfield.type=='render'">
                        <div v-html="cfield.render"></div>
                      </form-item-field>
                      <form-item-field :options="cfield" v-else>
                        <slot :name="cfield.slot" v-if="cfield.type=='dslot'"></slot>
                        <component
                          v-else
                          :is=" (cfield.type||'input') +'-field'"
                          v-model="values[cfield.prop]"
                          :options="cfield"
                          :style="fieldStyle(cfield)"
                        ></component>
                      </form-item-field>
                    </el-col>
                  </el-row>

                  <button-field class="d-button" v-else-if="field.type=='dbutton'" :options="field"></button-field>
                  <slot v-else-if="field.type=='slot'" :name="field.slot" :span="span(field)"></slot>
                  <form-item-field :options="field" v-else-if="field.type=='render'">
                    <div v-html="field.render"></div>
                  </form-item-field>

                  <form-item-field :options="field" v-else-if="field">
                    <slot :name="field.slot" v-if="field.type=='dslot'"></slot>
                    <component
                      v-else
                      :is=" (field.type||'input') +'-field'"
                      v-model="values[field.prop]"
                      :options="field"
                      :style="fieldStyle(field)"
                    ></component>
                  </form-item-field>
                </el-col>
              </el-row>
            </span>
          </span>
        </tip-title>
        <!-- 表格 -->
        <vue-el-table
          v-else-if="row.length==1&&row[0].type=='table'&&row[0].rowIndex==-1"
          :configs="row[0].configs"
          :values="defaultValues.values"
        ></vue-el-table>
        <!-- 表单 -->
        <el-row v-else-if="row[0].rowIndex==-1">
          <el-col v-for="(field, j) in row" :key="j" :span="span(field)">
            <el-row v-if="field.type=='multi'">
              <el-col v-for="(cfield,k) in field.childs" :key="k" :span="span(cfield)">
                <el-row v-if="cfield.type=='button'">
                  <el-col>
                    <button-field :options="cfield"></button-field>
                  </el-col>
                </el-row>
                <slot v-else-if="cfield.type=='slot'" :name="cfield.slot"></slot>
                <form-item-field :options="cfield" v-else-if="cfield.type=='render'">
                  <div v-html="cfield.render"></div>
                </form-item-field>
                <form-item-field :options="cfield" v-else>
                  <slot :name="cfield.slot" v-if="cfield.type=='dslot'"></slot>
                  <component
                    v-else
                    :is=" (cfield.type||'input') +'-field'"
                    v-model="values[cfield.prop]"
                    :options="cfield"
                    :style="fieldStyle(cfield)"
                  ></component>
                </form-item-field>
              </el-col>
            </el-row>

            <button-field class="d-button" v-else-if="field.type=='dbutton'" :options="field"></button-field>
            <slot v-else-if="field.type=='slot'" :name="field.slot" :span="span(field)"></slot>
            <form-item-field :options="field" v-else-if="field.type=='render'">
              <div v-html="field.render"></div>
            </form-item-field>

            <form-item-field :options="field" v-else-if="field">
              <slot :name="field.slot" v-if="field.type=='dslot'"></slot>
              <component
                v-else
                :is=" (field.type||'input') +'-field'"
                v-model="values[field.prop]"
                :options="field"
                :style="fieldStyle(field)"
              ></component>
            </form-item-field>
          </el-col>
        </el-row>
      </div>
    </el-form>
    <!-- 按钮 -->
    <div class="form-btns" v-if="buttons.length>0" :style="btnsStyle">
      <span v-for="button in buttons" :key="button['text']">
        <el-button
          :size="button['size']"
          :type="button['type']||'default'"
          :plain="button['plain']"
          :round="button['round']"
          :loading="button['loading']"
          :disabled="button['disabled']||loading"
          :icon="button['icon']"
          :autofocus="button['autofocus']"
          :native-type="button['native-type']"
          v-if="!button.hide"
          @click.native="button['click'](values)"
        >{{button['text']}}</el-button>
      </span>
    </div>
  </div>
</template>

<script>
import ENV from "./utils/env";

// 强制依赖element-ui
// import Vue from 'vue'
// 判断下是否有注册element组件
// if (!Vue.$ELEMENT) {
//   var ElementUI = require('element-ui')
//   require('element-ui/lib/theme-chalk/index.css')
//   Vue.use(ElementUI)
// }

export default {
  components: {
    InputField: () => import("./lib/InputField"),
    SelectField: () => import("./lib/SelectField"),
    RadioField: () => import("./lib/RadioField"),
    SwitchField: () => import("./lib/SwitchField"),
    CheckboxField: () => import("./lib/CheckboxField"),
    DateField: () => import("./lib/DateField"),
    TreeField: () => import("./lib/TreeField"),
    UploadField: () => import("./lib/UploadField"),
    EditorField: () => import("./lib/EditorField"),
    TipTitle: () => import("./lib/TipTitle"),
    UpdownloadField: () => import("./lib/UpdownloadField"),
    VueElTable: () => import("../VueElTable"),
    FormItemField: () => import("./lib/FormItemField"),
    ButtonField: () => import("./lib/ButtonField")
  },
  props: ["config", "defaultValues", "loading"],
  data() {
    return {
      values: {},
      getValues: {}
    };
  },
  created() {
    let setValues = this.config.values || {};
    let getValues = this.getValues;
    const _ENV = ENV;

    // 初始化values.key
    this.config.fields.forEach((field, i) => {
      if (
        field.type !== "tip" &&
        field.type !== "slot" &&
        field.type != "table" &&
        field.type != "multi" &&
        field.type != "button" &&
        field.type !== "dslot" &&
        field.type !== "render" &&
        field.type !== "dbutton"
      ) {
        if (!field.prop) {
          throw Error("field name 是必填的");
        } else if (getValues[field.prop]) {
          throw Error("field.prop: " + field.prop + " 重复");
        } else {
          getValues[field.prop] = "";
        }
      }
      if (!field.type) {
        field.type = "input";
      }
      if (this.config.disabled) {
        field.disabled = true;
      }
      if (field.type == "multi") {
        field.childs.forEach((child, i) => {
          if (!child.type) {
            child.type = "input";
          }
          if (this.config.disabled) {
            child.disabled = true;
          }
        });
      }
    });
    // 动态默认值
    Object.keys(setValues).forEach(key => {
      let value = setValues[key];
      if (value && value[0] === "#") {
        let indexs = value.substring(1).split(".");
        let _value = _ENV;
        indexs.some(index => {
          if (_value[index]) {
            _value = _value[index];
          } else {
            _value = "";
            return true;
          }
        });
        setValues[key] = _value;
      }
    });
    console.log(this.defaultValues);
    this.values = this.defaultValues;
  },
  // watch:{
  //   'config.values'(newValue,oldValue){
  //     let setValues = this.config.values || {}
  //     // 动态默认值
  //   Object.keys(setValues).forEach(key => {
  //     let value = setValues[key]
  //     if (value && value[0] === '#') {
  //       let indexs = value.substring(1).split('.')
  //       let _value = _ENV
  //       indexs.some(index => {
  //         if (_value[index]) {
  //           _value = _value[index]
  //         } else {
  //           _value = ''
  //           return true
  //         }
  //       })
  //       setValues[key] = _value
  //     }
  //   })
  //   this.values = Object.assign(this.getValues, setValues)
  //   }
  // },
  watch: {
    defaultValues(newValue, oldValue) {
      this.values = newValue;
    },
    "config.disabled"(newValue, oldValue) {
      if (newValue || newValue == false) {
        this.setDisabled(newValue);
      }
    }
  },
  computed: {
    form() {
      return this.$refs["form"];
    },
    fields() {
      let cols = this.config.cols;
      return this.config.fields.map(field => {
        this.setCol(field, cols);
        return field;
      });
    },
    buttons() {
      return (
        this.config.buttons ||
        [
          // {
          //   text: '提交',
          //   click: this.submit,
          //   type: 'primary'
          // },
          // {
          //   text: '重置',
          //   click: this.resetFields,
          //   type: 'danger'
          // }
        ]
      );
    },
    btnsStyle() {
      return {
        width: this.config["btns-width"],
        "text-align": this.config["btns-align"]
      };
    },
    // 一个简单的动态规划，初始化表单域布局
    layout() {
      let cols = this.config.cols || 2;
      let fields = this.fields;
      let lastIndex = fields.length - 1;

      var layout = [];
      // 填充空值
      var fill = function(row, num) {
        return row.concat(new Array(num).fill(0));
      };
      let rowIndex = -1;
      fields.reduce((row, field, index) => {
        let col = field.col;
        let count = row.reduce((sum, field) => sum + field.col, 0); // 求和
        let left = cols - count - col;
        if (field.type == "tip") {
          rowIndex = layout.length;
        }
        field.rowIndex = rowIndex;
        if (left >= 0) {
          // 还能容纳的情况
          row.push(field);
          if (index === lastIndex) {
            // 最后一个时，填充后直接添加一行
            layout.push(fill(row, left));
          } else {
            return row;
          }
        } else {
          // 容纳不下的情况，先添加当前行
          layout.push(fill(row, cols - count));
          // 然后处理下一行， 如果是最后一个，直接添加行
          if (index === lastIndex) {
            layout.push(fill([field], cols - col));
          } else {
            return [field];
          }
        }
      }, []);

      return layout;
    }
  },
  methods: {
    setDisabled(flag) {
      this.config.fields.map(field => {
        if (
          field.type !== "tip" &&
          field.type !== "slot" &&
          field.type != "table" &&
          field.type != "multi" &&
          field.type != "button"
        ) {
          field.disabled = flag;
        } else if (field.type == "multi") {
          field.childs.map(child => {
            if (
              child.type !== "tip" &&
              child.type !== "slot" &&
              child.type != "table" &&
              child.type != "multi" &&
              child.type != "button"
            ) {
              child.disabled = flag;
            }
          });
        }
      });
    },
    span(field) {
      return (field.col / this.config.cols) * 24;
    },
    setCol(field, cols) {
      if (this.config.disabled) {
        field.disabled = true;
      }
      // 预处理防止列配置超出范围
      if (
        field.type === "tip" ||
        field.type === "table" ||
        field.type == "editor"
      ) {
        field.col = cols;
      } else if (field.type == "multi") {
        field.childs.map(child => {
          this.setCol(child, cols);
        });
      } else {
        field.col = field.col > cols ? cols : field.col || 1;
      }
    },
    events(name) {
      if (name) {
        if (name[0] === "@") {
          this.$emit(name.substring(1), this.values);
        } else {
          this[name]();
        }
      }
    },
    fieldStyle(field) {
      return {
        width: field.width || "100%",
        "text-align": field.itenalign || "left"
      };
    },
    // 事件，提交表单，验证成功的话返回表单值
    submit() {
      this.validate(valid => {
        if (valid === true) {
          let values = this.values;

          this.$emit("submit", Object.assign({}, values));
        }
      });
    },
    validate(callback) {
      return this.$refs["form"].validate(callback);
    },
    validateField(fieldName, callback) {
      this.$refs["form"].validateField(fieldName, callback);
    },
    resetFields() {
      this.$refs["form"].resetFields();
    },
    clearFields() {
      for (let key in this.values) {
        this.values[key] = "";
      }
    },
    clearValidate() {
      this.$refs["form"].clearValidate();
    }
  }
};
</script>
<style lang="scss">
.vue-form[vm] {
  .form-body {
    .d-button {
      margin-left: 1rem;
      margin-top: 2px;
    }
  }
}
</style>


<style >
.vue-form[vm] {
  height: auto;
  width: 100%;
  display: flex;
  /*默认纵向布局*/
  flex-direction: column;
}
.vue-form[vm].top {
  flex-direction: column-reverse;
}
.vue-form[vm].left {
  flex-direction: row-reverse;
}
.vue-form[vm].right {
  flex-direction: row;
}
.vue-form[vm] .form-body {
  width: 100%;
}
.vue-form[vm] .form-notip {
  padding-top: 0.8rem;
}
.vue-form[vm] .el-col {
  text-align: left;
}
.vue-form[vm] .form-btns {
  width: 100%;
  margin-top: 1rem;
  text-align: right;
  padding-right: 15px;
}
.vue-form[vm] .form-btns .el-button {
  margin: 0 7px 1rem;
}
</style>