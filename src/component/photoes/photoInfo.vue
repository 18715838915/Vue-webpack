<template>
    
    <div class="photoInfo-container">
        <!-- 图片标题区域 -->
        <h3>{{photoInfo.title}}</h3>
        <p>
            <span>发表时间:{{photoInfo.add_time | dateFormat}}</span>
            <span>点击: {{photoInfo.click}}次</span>
        </p>
        <hr>

        <!-- 缩略图区域  -->
            <div class="thumbs">
      <!-- 注意：这里使用的 vue-preview 组件是版本 1.1.3,
           之前上课录屏时候, 所使用的 1.0.5 版本的 vue-preview 存在 Bug -->
         <vue-preview :slides="list"></vue-preview>
        </div>
        <!-- 图片内容区域 -->
        <div class="content" v-html="photoInfo.content"></div>
        <!-- 评论子组件区域 -->
        <remark-box :id="this.$route.params.id"></remark-box>
    </div>

</template>





<script>

// 导入评论子组件
import comment from '../subComponent/remark.vue'

export default {
    data(){
        return{
            photoId:this.$route.params.id,    //图片Id
            photoInfo:{},                   //图片详情对象
            list:[]                         //缩略图数组
        }
    },

    created(){
        this.getPhotoInfo();
        this.getThumbs();

    },

    methods:{
        getPhotoInfo(){
                 // 获取图片的详情
        this.$http.get('api/getimageInfo/' + this.photoId).then(result => {
        if (result.body.status === 0) {
          this.photoInfo = result.body.message[0]
        }
      })
        },


        getThumbs(){
                  // 获取缩略图
      this.$http.get('api/getthumimages/' + this.photoId).then(result => {
        if (result.body.status === 0) {
          
          // 循环每个图片数据，补全图片的宽和高
          result.body.message.forEach(item => {
            item.w = 600
            item.h = 600
            item.msrc = item.src
          })
          // 把完整的数据保存到 list 中
          this.list = result.body.message
          
        }
      })
        }
    },

    components:{
        //注册评论子组件
        "remark-box":comment
    }



}
</script>

<style lang="scss" scoped>
.photoinfo-container {
  padding: 3px;
  h3 {
    color: #26a2ff;
    font-size: 15px;
    text-align: center;
    margin: 15px 0;
  }
  .subtitle {
    display: flex;
    justify-content: space-between;
    font-size: 13px;
  }

  .content {
    font-size: 13px;
    line-height: 30px;
  }
}




</style>

<style lang="scss">
.thumbs {
  .my-gallery {
    display: flex;
    flex-wrap: wrap;
  }
  figure {
    width: 100px;
    height: 100px;
    margin: 10px;
    box-shadow: 0 0 8px #999;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
</style>

