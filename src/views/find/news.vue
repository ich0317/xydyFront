<template>
  <div id="news">
    <div v-if="list">
      <div class="news-block" v-for="(item,index) in list" :key="index">
        <div class="date">{{item.release_date | YMD}}</div>
        <div class="new-pic" @click="inDetail(item._id)">
          <img :src="item.img_url" width="100%">
        </div>
        <h3 class="news-title" @click="inDetail(item._id)">{{item.title}}</h3>
        <div
          class="news-intro"
        >{{item.brief}}</div>
        <div class="cont">
          <div class>阅读：{{item.views}}&nbsp;&nbsp;点赞：{{item.like}}</div>
        </div>
      </div>
      <div style="height:46px;"></div>
    </div>
    <noData :isShow="!list" title="小编太懒，还没有发布文章~"></noData>
    <fixedFoot></fixedFoot>
    <loading :isShow="loading"></loading>
  </div>
</template>
<script>
import fixedFoot from "@/components/fixedFooter/foot";
import noData from "@/components/noData/nodata";
import { getFindNew } from "@/api/api";
import loading from "@/components/loading/loading";
import { Toast } from 'mint-ui';
import { parseDateTime } from '@/utils/index';
export default {
  name: "",
  components: {
    fixedFoot,
    loading,
    noData
  },
  data() {
    return {
      loading: "",
      list:null
    };
  },
  methods: {
    getNews(){
      this.loading = true;
      getFindNew().then(res=>{
        let {code, msg, data} = res;
        if(code == 0){
          this.list = data;
        }else if(code == 1){
          console.warn(msg);
        }else{
          Toast(msg);
        }
        this.loading = false;
      })
    },
    inDetail(_id) {
      this.$router.push({
        name: "detail",
        query:{
          _id
        }
      });
    }
  },
  mounted(){
    this.getNews();
  },
  filters:{
    YMD:function(val){
      return parseDateTime(val,'{Y}-{M}-{D}')
    }
  }
};
</script>

<style lang="less" scoped>
@import "../../style/mixin";
#news {
  background: #f3f3f3;
  min-height: 100%;
  padding-top: 10px;
  .news-block {
    padding: 10px 15px;
    background: #fff;
    margin-bottom: 10px;
    .date {
      text-align: right;
      margin-bottom: 10px;
    }
  }
  .new-pic {
    margin-bottom: 10px;
  }
  .new-pic img {
    .borderRadius(4px);
    max-height: 172px;
  }
  .news-title {
    font-size: @bigSize;
    margin-bottom: 10px;
  }
  .news-intro {
    line-height: 1.6;
    .RowsCut(6);
    text-align: justify;
    margin-bottom: 10px;
  }
  .cont {
    color: @lightColor;
  }
}
</style>