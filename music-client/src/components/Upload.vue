<template>
  <div class="upload">
    <div class="section">
      <el-upload
        drag
        :action="uploadUrl()"
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :before-upload="beforeAvatarUpload">
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">将文件拖到此处，或<em>点击修改头像</em></div>
        <div class="el-upload__tip" slot="tip">注意只能上传jpg/png文件，且不超过10M哦！</div>
      </el-upload>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { mixin } from '../mixins'

export default {
  name: 'upload',
  mixins: [mixin],
  data () {
    return {
      imageUrl: ''
    }
  },
  computed: {
    ...mapGetters([
      'userId'
    ])
  },
  methods: {
    uploadUrl () {
      return `${this.$store.state.configure.HOST}/user/avatar/update?id=${this.userId}`
    },
    handleAvatarSuccess (res, file) {
      if (res.code === 1) {
        this.imageUrl = URL.createObjectURL(file.raw)
        this.$store.commit('setAvator', res.avator)
        this.$message({
          message: '修改成功',
          type: 'success'
        })
      } else {
        this.notify('修改失败', 'error')
      }
    },
    beforeAvatarUpload (file) {
      const isJPG = file.type === 'image/jpeg'
      const isLt2M = file.size / 1024 / 1024 < 10
      if (!isJPG) {
        this.$message.error('注意上传 JPG 格式的图片哦!')
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能要超过 10MB0哦!')
      }
      return isJPG && isLt2M
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/upload.scss';
</style>
