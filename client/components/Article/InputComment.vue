<template>
  <div id="input-comment" >

    <CameraIcon class="icon"/>
    
    <div v-if="!enlarge" @click="openText"  class="empty" >댓글을 남겨주세요.</div>
    <div v-else class="content-container">
      <textarea v-model="content" ref="commentText" placeholder="댓글을 남겨주세요." ></textarea>
      <div class="foot">
        <a @click.prevent="calne">취소</a>
        <a @click.prevent="uploadComment" :class="[content !== null && content !== '' && 'active' ]">등록</a>
      </div>
    </div>

  </div>
</template>

<script>
import { CameraIcon } from 'vue-feather-icons'

export default {
  props:{
    articleId:{
      type:String,
      required:true,
      
    }
  },
  data(){
    return{
      //true 입력할수있게 창이 펼쳐진 상태
      //false 창이 닫아진 상태
      enlarge:false,
      content:null,
      article:null,
    }
  },
  methods:{
    async uploadComment(){
      if(process.browser){
        const data = await this.$api.$post('/comment/create',{
          content:this.content,
          article:this.articleId
        })

        if(!data){
          return
        }
        this.enlarge = false
        this.content = null
      }
    },
    openText(){
      this.enlarge = true
      this.$nextTick(() =>{
        this.$refs.commentText.focus()
      })
    },
    calne(){
      this.enlarge = false
    }
    
  },
  components:{
    CameraIcon
  }
}
</script>

<style lang="scss" scoped>
#input-comment{
  display: flex;
  padding: 20px;
  border: 1px solid #d4d4d4;
  .empty{
    line-height: 28px;
    cursor: pointer;
    width: 100%;
  }
  .content-container{
    width: 100%;
  }
  textarea{
    resize: none;
    width: 100%;
    display: block;
    border: none;
    font-size: 16px;
    min-height: 100px;
    margin-bottom: 20px;
  }
  .icon{
    margin-right: 20px;
  }
  .foot{
    display: flex;
    justify-content: flex-end;
    gap: 20px;  
    a{
      color: #d4d4d4;
      &.active{
        color: #222;
      }
    }
  }
}

</style>