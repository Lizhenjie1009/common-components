<script>
/**
 * 使用范例
  <query-from 
    :conf="queryData" 
    @find='log' 
  />

数据格式: [[{}, {}],[]] 数组包含多行，每一行为一个数组为row，数组内嵌套多个对象为col

[
  [
    {
      label: '关系类型', type: 'select', name: 'gxtype', col: 4, colTit: 6, colData: 14, options: [
        { label: '男', value: 1 },
        { label: '女', value: 2 }
      ]
    },
    { label: '关系中文名称', type: 'input', name: 'ch', col: 4, colTit: 6, colData: 14 },
    { label: '关系英文名称', type: 'input', name: 'en', col: 4, colTit: 6, colData: 14 },
    { label: '开始节点', type: 'input', name: 'start', col: 4, colTit: 6, colData: 14 },
    { label: '目标节点', type: 'input', name: 'target', col: 4, colTit: 6, colData: 14 },
    { label: '查询', type: 'button', event: 'find', col: 4 }
  ]
]
 */

export default {
  name: "query-from",
  props: {
    conf: {
      require: true,
      type: Array
    }
  },
  data() {
    return {
      queryData: []
    }
  },
  created() {
    this.queryData = this.conf
  },
  render () {
    return (renderRows(this))

    // 每一行
    function renderRows(vm) {
      return (
        <div>
          {vm.queryData.map(row => {
            return (<el-row class='row-mb-20'>{renderColumn(row, vm)}</el-row>)
          })}
        </div>
      )
    }
    // 每一列
    function renderColumn(row, vm) {
      return row.map(col => {
        return (
          <el-col span={col.col}>
          {
            col.type !== 'button' ? 
            <el-col class="col-tit" span={col.colTit}>{col.label}</el-col> 
            : ''
          }
            <el-col span={col.colData}>{renderItem(col, vm)}</el-col>
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
              onInput={(e) => (vm.$set(element, 'value', e))}
              value={element.value}
            ></el-input>
          );
        case "select":
          return (
            <el-select
              size="small"
              onChange={(e) => (vm.$set(element, 'value', e))}
              value={element.value}
            >
              {element.options.map((e) => {
                return <el-option label={e.label} value={e.value}></el-option>;
              })}
            </el-select>
          );
        case "button":
          return (
            <el-button type="primary" class="blue bluesize" size="small" onClick={e => vm.$emit(element.event, dealData(vm))}>{element.label}</el-button>
          )
      }
    }

    // 处理数据
    function dealData (vm) {
      let resultObj = {}
      let filterButton = vm.queryData.map(row => row.filter(col => col.type !== 'button'))
      filterButton.forEach(row => row.forEach(col => {
        resultObj[col.name] = col.value
      }))
      return resultObj
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
</style>