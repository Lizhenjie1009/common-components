<template>
  <div class="d-from">
    <el-dialog
      :title="options.title ? options.title : '标题'"
      :visible.sync="dialogVisible"
      :width="options.width ? options.width : '40%'"
      :before-close="handleClose">
      <div class="d-content">
        <slot />
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handleClose">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
/**
 * 使用范例
  <dialog-from 
    :options="{
      title: '实体要素管理',
      width: '40%'
    }"
    :toggle.sync="dialogTool" 
    @save="save"
  >
    <div class="con">
      <h1>内容...</h1>
    </div>
  </dialog-from>
 */
export default {
  name: "d-from",
  props: {
    options: Object, // el-dialog 配置项
    toggle: {
      type: Boolean,
    }
  },
  data () {
    return {
      dialogVisible: false
    }
  },
  watch: {
    toggle (val) {
      this.dialogVisible = val
    }
  },
  created () {
    
  },
  methods: {
    handleClose () {
      this.dialogVisible = false
      this.$emit('update:toggle', false)
    },
    save () {
      this.handleClose()
      this.$emit('save')
    }
  }
}
</script>

<style lang="scss" scoped>
.d-from {
  
}
</style>