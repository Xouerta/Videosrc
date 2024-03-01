<template>
  <div class="p-grid p-justify-center p-align-center">
    <div class="p-col-12 p-md-6">
      <div class="p-d-flex p-jc-center">
        <div class="upload-area p-d-flex p-jc-center p-ai-center" @dragover.prevent @drop="handleDrop">
          <i class="pi pi-upload"></i>
          <span>拖拽视频到此处上传</span>
        </div>
      </div>
      <div class="p-d-flex p-jc-center p-mt-2">
        <input type="file" ref="fileInput" @change="handleFileInput" style="display: none">
        <Button type="button" label="选择视频文件" icon="pi pi-folder-open" class="p-button-raised p-button-info" @click="openFileInput" />
      </div>
      <div v-if="video" class="p-d-flex p-jc-center p-mt-2">
        <Button type="button" label="上传视频" icon="pi pi-upload" class="p-button-raised p-button-success" @click="uploadVideo" />
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import { Button } from 'primevue/button';
import 'primevue/resources/themes/saga-blue/theme.css';
import 'primevue/resources/primevue.min.css';
import 'primeicons/primeicons.css';

export default {
  components: {
    Button
  },
  data() {
    return {
      video: null
    };
  },
  methods: {
    openFileInput() {
      this.$refs.fileInput.click();
    },
    handleFileInput(event) {
      const file = event.target.files[0];
      this.video = file;
    },
    handleDrop(event) {
      event.preventDefault();
      const file = event.dataTransfer.files[0];
      this.video = file;
    },
    uploadVideo() {
      // 处理视频上传逻辑，可以发送视频文件到后端等
      console.log('上传视频:', this.video);
      // 上传完成后，清空视频
      this.video = null;
    }
  }
};
</script>

<style scoped>
.upload-area {
  border: 2px dashed #ccc;
  padding: 20px;
  cursor: pointer;
  width: 100%;
  height: 200px;
  text-align: center;
}

.upload-area i {
  font-size: 3rem;
  margin-bottom: 10px;
}
</style>
