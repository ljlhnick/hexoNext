---
title: 基于vue框架以及网易云API实现的小项目
categories: 
  - Vue
  - Jest
tags:
  - nodejs
  - Vue
  - React
---
<!--more-->
## 技术方案
github地址： **https://github.com/ljlhnick/vue-wangyiyun**  喜欢欢迎点个start，支持下我
技术方案：vue+vue-router+axios+typeScript+element-ui+vue-property-decorator（class方式写组件）
## 模块划分
歌单列表，歌单，分页，播放详情，个人资料提交
## 代码class编程
```
<script lang="ts">
import { formatDate } from "./date.js";
import TableList from "../tableList.vue";
import axios from "axios";
import { Vue, Component, Watch } from "vue-property-decorator";
@Component({
  components: {
    TableList
  },
  filters: {
    formatDate(time: Date) {
      const date = new Date(time);
      return formatDate(date, "yyyy-MM-dd");
    }
  }
})
export default class SongFirst extends Vue {
  subdesc = "";
  sheepList = [];
  totalCount = 0;
  currentPage = 1;

  created() {
    this.get();
  }

  get() {
    if (typeof this.$route.query.songId === "undefined") {
      return;
    }
    axios({
      url: "https://api.imjad.cn/cloudmusic/",
      method: "get",
      params: {
        type: "playlist",
        id: this.$route.query.songId,
        offest: this.currentPage - 1
      }
    }).then(res => {
      this.subdesc = res.data.playlist;
      this.sheepList = res.data.playlist.tracks;
      this.totalCount = res.data.playlist.trackCount;
    });
  }

  changePage(currentPage: number) {
    this.currentPage = currentPage;
    this.get();
  }

  @Watch("$route", { immediate: false, deep: false })
  pageChange() {
    this.get();
  }
}
</script>
```
## 功能图
主页
![主页](https://img-blog.csdnimg.cn/2020052317382318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
歌单
![歌单](https://img-blog.csdnimg.cn/20200523173908904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
播放详情
![播放详情](https://img-blog.csdnimg.cn/20200523173941241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
我的（表单验证）
![表单验证](https://img-blog.csdnimg.cn/20200523174013810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)