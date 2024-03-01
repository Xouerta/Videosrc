<template>
  <div>
    <h2>视频后台管理系统</h2>
    <div v-for="video in videos" :key="video.id">
      <p>{{ video.title }}</p>
      <button @click="deleteVideo(video.id)">删除</button>
      <button @click="approveVideo(video)">通过</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      videos: [],
    };
  },
  mounted() {
    this.fetchVideos();
  },
  methods: {
    fetchVideos() {
      axios.get('/approve/media')
          .then(response => {
            this.videos = response.data;
          })
          .catch(error => {
            console.error('Failed to fetch videos:', error);
          });
    },
    deleteVideo(videoId) {
      axios.delete(`/approve/media/${videoId}`)
          .then(() => {
            // 删除成功后，重新获取视频列表
            this.fetchVideos();
          })
          .catch(error => {
            console.error('Failed to delete video:', error);
          });
    },
    approveVideo(video) {
      // 编辑视频的逻辑，可以跳转到编辑页面或者弹出编辑框
      console.log('Approving video:', video);
    },
  },
};
</script>
