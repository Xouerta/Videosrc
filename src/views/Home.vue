<template>
  <div class="home" v-if="category">
    <nav-bar></nav-bar>
    <video-from></video-from>
    <van-card
        desc="描述信息"
        title="视频标题"
        thumb="@/assets/1.png"
    >
      <template #tags>
        <van-tag plain type="primary">视频</van-tag>
        <van-tag plain type="primary">图文</van-tag>
      </template>
      <template #footer>
        <van-button size="mini">喜欢</van-button>
        <van-button size="mini">播放</van-button>
      </template>
    </van-card>
    <van-card
        view="200"
        desc="描述信息"
        title="图文标题"
        thumb="@/assets/2.png"
    >
      <template #tags>
        <van-tag plain type="primary">视频</van-tag>
        <van-tag plain type="primary">图文</van-tag>
      </template>
      <template #footer>
        <van-button size="mini">喜欢</van-button>
        <van-button size="mini">播放</van-button>
      </template>
    </van-card>
    <van-card
        view="200"
        desc="描述信息"
        title="视频标题"
        thumb="@/assets/2.png"
    >
      <template #tags>
        <van-tag plain type="primary">视频</van-tag>
        <van-tag plain type="primary">图文</van-tag>
      </template>
      <template #footer>
        <van-button size="mini">喜欢</van-button>
        <van-button size="mini">播放</van-button>
      </template>
    </van-card>
    <van-card
        view="200"
        desc="描述信息"
        title="图文标题"
        thumb="@/assets/2.png"
    >
      <template #tags>
        <van-tag plain type="primary">视频</van-tag>
        <van-tag plain type="primary">图文</van-tag>
      </template>
      <template #footer>
        <van-button size="mini">喜欢</van-button>
        <van-button size="mini">播放</van-button>
      </template>
    </van-card>
    <van-pagination
        v-model="currentPage"
        :total-items="125"
        :show-page-size="6"
        force-ellipses
    />


    <div class="categorytab">
      <div class="category-ico" @click="$router.push('/editcategory')"><van-icon name="setting-o" /></div>
      <van-tabs v-model="active" swipeable sticky animated>
        <van-tab v-for="(item,index) in category" :key="index" :title="item.title">
          <van-list
            v-model="item.loading"
            :immediate-check="false"
            :finished="item.finished"
            finished-text="我也是有底线的"
            @load="onLoad"
          >
            <div class="detailparent">
              <cover
                class="detailitem"
                :detailitem="categoryitem"
                v-for="(categoryitem,categoryindex) in item.list"
                :key="categoryindex"
              />
            </div>
          </van-list>
        </van-tab>
      </van-tabs>
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/common/Navbar.vue";
import cover from "./cover";
import VideoFrom from "@/components/common/videoFrom.vue";
import vant from "vant";
export default {
  data() {
    return {
      category: [],
      active: 0
    };
  },
  components: {
    VideoFrom,
    NavBar,
    cover
  },
  activated() {
    if(localStorage.getItem('newCat')) {
        let newCat = JSON.parse(localStorage.getItem('newCat'))
        this.category = this.changeCategory(newCat)
        this.selectArticle();
    }
  },
  methods: {
    async selectCategory() {
      if(localStorage.getItem('newCat')) {
        return
      }
      const res = await this.$http.get("/category");
      this.category = this.changeCategory(res.data);
      this.selectArticle();
    },
    changeCategory(data) {
      const category1 = data.map((item, index) => {
        item.list = [];
        item.page = 0;
        item.finished = false;
        item.loading = true;
        item.pagesize = 10;
        return item;
      });
      return category1;
    },
    async selectArticle() {
      const categoryitem = this.categoryItem();
      const res = await this.$http.get("/detail/" + categoryitem._id, {
        params: {
          page: categoryitem.page,
          pagesize: categoryitem.pagesize
        }
      });
      categoryitem.list.push(...res.data);
      categoryitem.loading = false;
      if (res.data.length < categoryitem.pagesize) {
        categoryitem.finished = true;
      }
    },
    onLoad() {
      const categoryitem = this.categoryItem();
      setTimeout(() => {
        categoryitem.page += 1;
        this.selectArticle();
      }, 1000);
    },
    categoryItem() {
      const categoryitem = this.category[this.active];
      return categoryitem;
    }
  },
  watch: {
    active() {
      const categoryitem = this.categoryItem();
      if (!categoryitem.list.length) {
        this.selectArticle();
      }
    }
  },
  created() {
    this.selectCategory();
  }
};
</script>

<style lang="less">
.home {
  background-color: white;
}
.detailparent {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  .detailitem {
    margin: 1.389vw 0;
    width: 45%;
  }
}
.categorytab{
  position: relative;
  .category-ico{
    position: absolute;
    z-index: 5;
    right: 0;
    top: 1.944vw;
    padding: 1.389vw 2.778vw;
    background-color: white;
  }
}

</style>