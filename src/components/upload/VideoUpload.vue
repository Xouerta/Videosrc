<template>
  <div class="video-upload">
    <van-uploader
        :file-list="fileList"
        accept="video/*"
        multiple
        drag
        :before-read="beforeRead"
        :after-read="afterRead"
    >
      <template #upload="props">
        <div class="upload-demo">
          <van-icon class="icon" name="photograph" />
          <span>将视频文件拖到此处，或<em>点击上传</em></span>
        </div>
      </template>
    </van-uploader>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileList: []
    };
  },
  methods: {
    beforeRead(file) {
      // 可以在这里添加一些上传文件的前置处理逻辑，比如文件大小限制等
      console.log('上传文件:', file);
      return true; // 返回 true 表示允许上传，返回 false 表示不允许上传
    },
    afterRead(file) {
      // 文件读取完成后的回调函数
      console.log('文件读取完成:', file);
    }
  }
};
</script>

<style>
.video-upload {
  width: 100%;
  height: 100%;
}

.upload-demo {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 200px;
  border: 1px dashed #e5e5e5;
  background-color: #fafafa;
  color: #999;
}

.upload-demo .icon {
  font-size: 48px;
  margin-bottom: 10px;
}
</style>
