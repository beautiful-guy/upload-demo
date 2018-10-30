<template>
  <div class="hello">
      <router-link to="/secondPage">
          <h1>点我跳转到第二页</h1>
      </router-link>
      <!--<form method="post" action="https://upload-z1.qiniup.com"-->
            <!--enctype="multipart/form-data">-->
          <!--<input type="text" name="token" v-model="formData.token">-->
          <!--<input name="file" type="file" />-->
          <!--<br>-->
          <!--<br>-->
          <!--<button>上传</button>-->
      <!--</form>-->
        <!--自定义上传图片-->
      <el-upload
              class="avatar-uploader"
              action="https://upload-z1.qiniup.com"
              :show-file-list="false"
              :data="formData"
              :on-success="handleAvatarSuccess">
          <img v-if="imageUrl" :src="imageUrl" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
      </el-upload>
  </div>
</template>

<script>
    import qiniu from 'qiniu';
export default {
  name: 'HelloWorld',
  data(){
    return{
      formData:{
        token:''
      },
      imageUrl:''
    }
  },
  methods:{
    getToken(){
      var accessKey = 'qhSYKUkzYoN_n_DN6ZRuZv2oYWC6wr-bL17qWMnC';
      var secretKey = 'neAgkvFVysfkOzOfJlYGFCLWjJvJaTQEV5BSljrT';
      var mac = new qiniu.auth.digest.Mac(accessKey, secretKey);
      var options = {
        scope: 'lemonade',
        returnBody: '{"key":"$(key)","hash":"$(etag)","url":"http://phef4dnvz.bkt.clouddn.com/$(key)"}'
      };
      var putPolicy = new qiniu.rs.PutPolicy(options);
      var uploadToken=putPolicy.uploadToken(mac);
      this.formData.token = uploadToken;
    },
    handleAvatarSuccess(res){
      console.log(res)
    }
  },
  created(){
    this.getToken();
  }
}
</script>

<style scoped>
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
        border-color: #409EFF;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        line-height: 178px;
        text-align: center;
    }
    .avatar {
        width: 178px;
        height: 178px;
        display: block;
    }
</style>
