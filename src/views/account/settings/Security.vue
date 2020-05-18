<template>
   <div class="account-settings-info-view">
    <a-row :gutter="16">
      <a-col :md="24" :lg="16">

        <a-form layout="inline">
          <a-form-item
            label="右上角配置1"
          >
            <a-input   placeholder="" />
          </a-form-item>
         <a-form-item
            label="连接URL"
           
          >
            <a-input placeholder="" />
          </a-form-item>

          <a-form-item
            label="右上角配置2"
          >
            <a-input placeholder="" />
          </a-form-item>
          <a-form-item
            label="连接URL"
          >
            <a-input placeholder="" />
          </a-form-item>
           <a-form-item
            label="右上角配置3"
          >
            <a-input placeholder="" />
          </a-form-item>
          <a-form-item
            label="连接URL"
          >
            <a-input placeholder="" />
          </a-form-item>
        
         
          <a-form-item>
            <a-button type="primary">提交</a-button>
          </a-form-item>
        </a-form>

      </a-col>
      
    </a-row>

    <avatar-modal ref="modal" @ok="setavatar"/>

  </div>
</template>

<script>
export default {
  data () {
    return {
      data: [
        { title: '账户密码', description: '当前密码强度', value: '强', actions: { title: '修改', callback: () => { this.$message.info('This is a normal message') } } },
        { title: '密保手机', description: '已绑定手机', value: '138****8293', actions: { title: '修改', callback: () => { this.$message.success('This is a message of success') } } },
        { title: '密保问题', description: '未设置密保问题，密保问题可有效保护账户安全', value: '', actions: { title: '设置', callback: () => { this.$message.error('This is a message of error') } } },
        { title: '备用邮箱', description: '已绑定邮箱', value: 'ant***sign.com', actions: { title: '修改', callback: () => { this.$message.warning('This is message of warning') } } },
        { title: 'MFA 设备', description: '未绑定 MFA 设备，绑定后，可以进行二次确认', value: '', actions: { title: '绑定', callback: () => { this.$message.info('This is a normal message') } } }
      ]
    }
  }
}
</script>

<style scoped>

</style>
