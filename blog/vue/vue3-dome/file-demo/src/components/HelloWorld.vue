<template>
  <div>
    <Input @uploadedS='uploadedS'></Input>
  </div>
</template>

<script>
import Input from './input.vue'
export default {
  name: 'HelloWorld',
  props: {
    
  },
  components: {
    Input
  },
  data() {
    return {

    }
  },
  methods: {
    uploadedS() {
      console.log('文件上传成功函数')
    }
  }
}
</script>
