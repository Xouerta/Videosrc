<template>
  <div>
    <h2>视频管理系统</h2>
    <van-card v-for="video in videos" :key="video.id">
      <template #title>
        {{ video.title }}
      </template>
      <template #desc>
        {{ video.description }}
      </template>
      <template #footer>
        <van-button type="danger" @click="deleteVideo(video.id)">删除</van-button>
        <van-button type="primary" @click="editVideo(video)">修改</van-button>
      </template>
    </van-card>
  </div>
</template>

<script>
import { ref } from 'vue';
import { Toast } from 'vant';
import axios from 'axios';

export default {
  setup() {
    const videos = ref([]);

    const fetchVideos = () => {
      axios.get('/api/videos')
          .then(response => {
            videos.value = response.data;
          })
          .catch(error => {
            console.error('Failed to fetch videos:', error);
          });
    };

    const deleteVideo = (videoId) => {
      axios.delete(`/api/videos/${videoId}`)
          .then(() => {
            // 删除成功后，重新获取视频列表
            fetchVideos();
            Toast.success('删除成功');
          })
          .catch(error => {
            console.error('Failed to delete video:', error);
            Toast.fail('删除失败');
          });
    };

    const editVideo = (video) => {
      // 编辑视频的逻辑，可以跳转到编辑页面或者弹出编辑框
      console.log('Editing video:', video);
    };

    // 初始化时获取视频列表
    fetchVideos();

    return {
      videos,
      deleteVideo,
      editVideo,
    };
  },
};
</script>
