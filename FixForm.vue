<script>
/**
* 使用范例
  <fix-form 
    :conf="formOptions" 
    :form="form" 
  />

formData数据格式: [[{}, {}],[]] 数组包含多行，每一行为一个数组为row，数组内嵌套多个对象为col

  formOptions: { 
    formConfig: { // 配置数据
      labelWidth: '100px',
      prop: true,
      rules: {
        gxtype: [
          { required: true, message: '请选择', trigger: 'change' }
        ],
        start: [
          { required: true, message: '请输入', trigger: 'blur' }
        ],
        target: [
          { required: true, message: '请输入', trigger: 'blur' }
        ]
      }
    },
    formData: [ // 模板格式数据
      [
        {
          label: '关系类型', type: 'select', name: 'gxtype', col: 24, options: [
            { label: '男', value: 1 },
            { label: '女', value: 2 }
          ]
        },
      ],
      [{ label: '开始节点', type: 'input', name: 'start', col: 24 }],
      [{ label: '目标节点', type: 'input', name: 'target', col: 24 }],
    ]
  }
 */

export default {
  name: "fix-form",
  props: {
    form: Object, // 表单对象
    conf: Object // 配置文件
  },
  data() {
    return {
      formData: []
    }
  },
  created() {
    this.formData = this.conf.formData
  },
  render () {
    return (renderRows(this))

    // 每一行
    function renderRows(vm) {
      return (
        <el-form
          class="fixForm"
          ref="fixForm"
          props={{
            model: vm.form
          }}
          rules={vm.conf.formConfig.rules ? vm.conf.formConfig.rules : {}}
          label-width={vm.conf.formConfig.labelWidth}
          label-position="left"
        >
          {vm.formData.map(row => {
            return (<el-row class='row-mb-20'>{renderColumn(row, vm)}</el-row>)
          })}
        </el-form>
      )
    }
    // 每一列
    function renderColumn(row, vm) {
      return row.map(col => {
        return (
          <el-col span={col.col}>
            {vm.conf.formConfig.prop
              ? <el-form-item label={col.label} prop={col.name}>
                  {renderItem(col, vm)}
                </el-form-item>
              :<el-form-item label={col.label} >
                  {renderItem(col, vm)}
                </el-form-item>
            }
          </el-col>
        )
      })
    }
    // 渲染元素
    function renderItem(element, vm) {
      switch (element.type) {
        case "input":
          return (
            <el-input
              size="small"
              onInput={(e) => (vm.$set(vm.form, element.name, e))}
              value={vm.form[element.name]}
            ></el-input>
          );
        case "select":
          return (
            <el-select
              size="small"
              onChange={(e) => (vm.$set(vm.form, element.name, e))}
              value={vm.form[element.name]}
              placeholder="请选择"
            >
              {element.options.map((e) => {
                return <el-option label={e.label} value={e.value}></el-option>;
              })}
            </el-select>
          );
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.col-tit {
  height: 100%;
  line-height: 32px;
}
.row-mb-20 {
  margin-bottom: 20px;
}

/deep/.el-form-item {
  margin-bottom: 20px;
}

/deep/.el-input {
  width: 60%;
}
/deep/.el-select {
  width: 60%;
}
/deep/.el-select .el-input {
  width: 100%;
}
</style>