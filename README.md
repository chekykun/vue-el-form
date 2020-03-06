# vue-el-form
vue element table
#使用方法
需要引入element-ui和lodash.js
引入
```
import VueElForm from './VueElForm'
```
#例子
```

<vue-el-form ref="vueElForm"
                   :config="formConfigs"
                   :defaultValues="formValues">
      </vue-el-form>
data:{
    return {
        formConfigs:{
        cols: 3,
        'label-width': '5rem',
        'width': '80%',
        disabled: this.disable,
        rules: userDetailRules,
        fields: [
          { type: 'tip', title: '基本信息' },
          {            type: 'multi', col: 2, childs: [
              { label: '姓名', prop: 'name', col: 3 },
              { label: '单位', prop: 'companyCode', type: 'select', valueKey: 'dictDataId', labelKey: 'dictDataName', options: this.companySelectOptions, col: 3 },
              { label: '联系电话', prop: 'mobilePhone', col: 3 },
              { label: '职务', prop: 'positionId', type: 'select', valueKey: 'dictDataId', labelKey: 'dictDataName', options: this.positionSelectOptions, col: 4 }
            ]          },
          {            label: '', prop: 'url', type: 'upload', action: '/app/file/upload', 'show-file-list': false,
            'on-success': this.handleAvatarSuccess, 'list-type': 'picture-card', 'before-upload': this.beforeAvatarUpload, cols: 2          },
          { label: '职责', dtype: 'textarea', prop: 'jobFunction', col: 3 },
          {            label: '人员角色', type: 'checkbox', prop: 'personRole', col: 3, checkboxs: [
              { label: 'isGrobalDuty', name: 'personRole', text: '全局值班' },
              { label: 'isWaterDuty', name: 'personRole', text: '水量调度值班' },
              { label: 'isMsgRec', name: 'personRole', text: '短信接收人' }
            ]          },
          { label: '备注', dtype: 'textarea', prop: 'note', col: 3 }
        ],
      },
      formValues:{
        companyCode: "",
        id: "",
        isDuty: "",
        isDutyMsg: {},
        isMsgRec: "",
        isMsgRecMsg: {},
        jobFunction: "",
        mobilePhone: "",
        name: "",
        note: "",
        positionId: "",
        url: "",
        personRole: []
      }
    }
}
```
效果：


