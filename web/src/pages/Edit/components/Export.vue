<template>
  <el-dialog
    class="nodeDialog"
    :title="$t('export.title')"
    :visible.sync="dialogVisible"
    width="700px"
  >
    <div>
      <div class="nameInputBox">
        <span class="name">{{ $t('export.filename') }}</span>
        <el-input
          style="width: 300px"
          v-model="fileName"
          size="mini"
        ></el-input>
        <el-checkbox
          v-show="['smm', 'json'].includes(exportType)"
          v-model="widthConfig"
          style="margin-left: 12px"
          >{{ $t('export.include') }}</el-checkbox
        >
      </div>
      <el-radio-group v-model="exportType" size="mini">
        <el-radio-button label="smm"
          >{{ $t('export.dedicatedFile') }}（.smm）</el-radio-button
        >
        <el-radio-button label="json"
          >{{ $t('export.jsonFile') }}（.json）</el-radio-button
        >
        <el-radio-button label="png"
          >{{ $t('export.imageFile') }}（.png）</el-radio-button
        >
        <el-radio-button label="svg"
          >{{ $t('export.svgFile') }}（.svg）</el-radio-button
        >
        <el-radio-button label="pdf"
          >{{ $t('export.pdfFile') }}（.pdf）</el-radio-button
        >
      </el-radio-group>
      <div class="tip">{{ $t('export.tips') }}</div>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="cancel">{{ $t('dialog.cancel') }}</el-button>
      <el-button type="primary" @click="confirm">{{
        $t('dialog.confirm')
      }}</el-button>
    </span>
  </el-dialog>
</template>

<script>
/**
 * @Author: 王林
 * @Date: 2021-06-24 22:53:54
 * @Desc: 导出
 */
export default {
  name: 'Export',
  data() {
    return {
      dialogVisible: false,
      exportType: 'smm',
      fileName: '思维导图',
      widthConfig: true
    }
  },
  created() {
    this.$bus.$on('showExport', () => {
      this.dialogVisible = true
    })
  },
  methods: {
    /**
     * @Author: 王林
     * @Date: 2021-06-22 22:08:11
     * @Desc: 取消
     */
    cancel() {
      this.dialogVisible = false
    },

    /**
     * @Author: 王林
     * @Date: 2021-06-06 22:28:20
     * @Desc:  确定
     */
    confirm() {
      this.$bus.$emit(
        'export',
        this.exportType,
        true,
        this.fileName,
        this.widthConfig
      )
      this.$notify.info({
        title: '消息',
        message: '如果没有触发下载，请检查是否被浏览器拦截了'
      })
      this.cancel()
    }
  }
}
</script>

<style lang="less" scoped>
.nodeDialog {
  .nameInputBox {
    margin-bottom: 20px;

    .name {
      margin-right: 10px;
    }
  }

  .tip {
    margin-top: 10px;
  }
}
</style>
