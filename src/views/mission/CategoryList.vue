<template lang='pug'>
  .page-container
    .page-top-tip 平台名称：{{$route.query.pname}}
    .page-operatebar
      el-button(type='primary' @click='isDialogShow = true') 添加任务类型
    el-table(:data='list' border stripe)
      el-table-column(type='index')
      el-table-column(label='任务类型名称	' prop='tname')
      el-table-column(label='单价（元）' prop='tunit')
      el-table-column(label='兼职单价（元）' prop='receiverUnit')
      el-table-column(label='操作' prop='')
        template(slot-scope='{row}')
          el-button(type='text' @click='$router.push({name: "childCategoryList", query: {fatherTaskType: row.tid, tname: row.tname}})') 查看子类型
          el-button(type='text' @click='delPlatform(row)') 删除
    el-dialog(:visible.sync='isDialogShow' title='新建平台' width='50%')
      el-form(label-width='120px' label-position='right' :model='form' :rules='rules' ref='form')
        el-form-item(label='名称:' prop='taskTypeName')
          el-input(placeholder='请输入名称' v-model='form.taskTypeName' clearable).w300
        el-form-item(label='单价' prop='taskTypeUnit')
          el-input(placeholder='请输入单价' v-model='form.taskTypeUnit' clearable).w300
        el-form-item(label='兼职单价' prop='receiverUnit')
          el-input(placeholder='请输入兼职单价' v-model='form.receiverUnit' clearable).w300
      .p-foot(slot='footer')
        el-button(type='primary' @click='submit') 创建
        el-button(@click='isDialogShow=false') 取消
</template>
<script>
export default {
  data() {
    return {
      ENV,
      form: {
        taskTypeName: '',
        taskTypeUnit: '',
        receiverUnit: '',
        taskTypePlatform: ''
      },
      rules: {
        taskTypeName: [
          {required: true, message: '请输入平台名称'}
        ],
        taskTypeUnit: [
          {required: true, message: '请输入单价'}
        ],
        receiverUnit: [
          {required: true, message: '请输入兼职单价'}
        ]
      },
      fileList: [],
      isDialogShow: false,
      isDialogShow1: false,
      form1: {
        platformName: '',
        platformId: ''
      },
      list: [],
      action: `${ENV}/Task/uploadMedia`,
    }
  },
  methods: {
    delPlatform(row) {
      this.$confirm('此操作将删除该类型, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        const res = await API.mission.delPC({
          taskTypeId: row.tid
        })

        if(res.data.code === 100) {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.getPCList()
        }
      })
    },
    submit() {
      this.$refs.form.validate(async valid => {
        if(valid) {
          const res = await API.mission.addPC(this.form)

          if(res.data.code === 100) {
            this.$message.success('添加成功')
            this.isDialogShow = false
            this.form = {
              taskTypeName: '',
              taskTypeUnit: '',
              receiverUnit: '',
              taskTypePlatform: this.$route.query.platformId
            }
            this.getPCList()
          }
        }
      })
    },
    async getPCList() {
      this.form.taskTypePlatform = this.$route.query.platformId

      const res = await API.mission.getPCList({
        platformId: this.form.taskTypePlatform
      })

      if(res.data.code === 100) {
        this.list = res.data.data.taskTypeList
      }
    }
  },
  mounted() {
    this.getPCList()
  }
}
</script>
<style lang='less' scoped>

</style>