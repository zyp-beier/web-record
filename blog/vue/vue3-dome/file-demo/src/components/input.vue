<template>
  <div class="file-wrap">
    <label for="file">请选择文件</label>
    <div class="file-list">
      <div v-for="(item, index) of fileList" :key="index" class="previewImg">
        <div>{{item.name}}</div>
        <div class="pre">
          <div class="delete" @click="handleDeleteFile(index, 'file')">删除</div>
          <div class="preview" @click="handlePreviewFile(index, 'file')">预览</div>
        </div>
      </div>
      <div class="img-wrap">
        <div v-for="(item, index) of imageUrl" :key="index" class="previewImg">
          <div class="file-img">
            <img :src='item' alt="">
          </div>
          <div class="pre">
            <div class="delete" @click="handleDeleteFile(index, 'image')">删除</div>
            <div class="preview" @click="handlePreviewFile(index, 'image')">预览</div>
          </div>
        </div>
      </div>
    </div>
    <div class="file-main">
      <input type="file" id="file" :accept="accept" :multiple='multiple' @change="fileChange" >
      <div class="file-style"><div class="icon">+</div></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "input",
  data() {
    return {
      fileList: [],
      imageUrl: [],
      previewCard: false
    }
  },
  props: {
    accept: {
      type: String,
      default: 'image/*'
    },
    files: {
      type: Object,
    },
    multiple: {  // 多个上传
      type: Boolean,
      default: false
    }
  },
  methods: {
    // 文件上传
    fileChange(event) {
      if (event.target.files.length) {
        this.getFile(event.target.files)
      }
    },
    getResult(reader) {
      return new Promise(res => {
        reader.onload = function() {
          res(reader.result)
        }
      })
    },
    // 赋值
    async getFile(files) {
      let filelist = []
      let imageUrl = []
      for (let item of files) {
        const type = item.type
        let b = type.substring(type.indexOf('/') + 1)
        if (b === 'jpg' || b === 'png' || b === 'jpeg') {
          const reader = new FileReader()
          reader.readAsDataURL(item)
          const data =  await this.getResult(reader)
          imageUrl.push(data)
        } else {
          filelist.push(item)
        }
      }
      this.imageUrl = this.imageUrl.concat(imageUrl)
      this.fileList = this.fileList.concat(filelist)
      this.$emit('uploadedS')
    },
    // 删除文件
    handleDeleteFile(index, type) {
      if (type === 'image') {
        this.imageUrl.splice(index, 1)
      } else if (type === 'file') {
        this.fileList.splice(index, 1)
      }
      console.log(this.fileList)
      return
    },
    // 预览文件
    handlePreviewFile(index, type) {
      if (type === 'image') {
        this.previewFile = this.imageUrl[index]
      }else if (type === 'file') {
        this.previewFile = this.fileList[index]
      }
      this.previewCard = true
      return
    },   
  }
}
</script>

<style scoped>
  .file-wrap {
    display: flex;
  }
  label {
    font-size: 14px;
    color: rgb(82, 81, 81);
    width: 100px;
  }
  .file-main {
    position: relative;
  }
  #file {
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0;
    z-index: 999;
    width: 200px;
    height: 150px;
  }
  .file-style {
    position: absolute;
    top: 0;
    left: 0;
    width: 200px;
    height: 150px;
    border: 1px dotted #ccc;
    border-radius: 20px;
  }
  .icon {
    font-size: 40px;
    font-weight: 100;
    line-height: 150px;
    text-align: center;
    color: #ccc;
  }
   .file-list {
    display: flex;
    align-items: center;
  }
  .img-wrap {
    display: flex;
  }
  .previewImg {
    position: relative;
    width: 200px;
    height: 150px;
    overflow: hidden;
    display: flex;
    align-items: center;
    border-radius: 20px;
    border: 1px dotted #ccc;
  }
  .file-img {

  }
  .file-img img {
    width: 100%;
  }
  .pre {
    position: absolute;
    top: 0;
    left: 0;
    width: 200px;
    height: 150px;
    border-radius: 20px;
    background: rgba(0, 0, 0, .5);
    opacity: 0;
    display: flex;
    align-items: center;
  }
  .pre:hover {
    opacity: 100;
  }
  .delete, .preview{
    height: 50%;
    width: 50%;
    padding: 20px;
    box-sizing: border-box;
    color: white;
    cursor:pointer
  }
</style>