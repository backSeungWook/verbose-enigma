<template>
  <div class="main-container">
    <main>
      <searchbar />
      <best-board-card v-if="mainContent[0]" :articleList="mainContent[0].content" />
      <div class="board-card-container">
        <board-card 
          v-for ="b in mainContent" 
          :key="b.slug" 
          :title="b.title"
          :slug="b.slug" 
          :articleList="b.content"
        /> 
       
      </div>
    </main>
    <realtime-famous-company />
  </div>
</template>

<script>
import BoardCard from '../components/Main/BoardCard.vue'
import BestBoardCard from '@/components/Main/BestBoardCard'
// import Searchbar from '../components/Searchbar.vue'
import RealtimeFamousCompany from '@/components/Main/RealtimeFamousCompany'


export default {
  data(){
    return{
      mainContent:[]
    }
  },
  components: { 
    BoardCard, 
    //Searchbar ,
    BestBoardCard,
    RealtimeFamousCompany,
  
  },
  created(){
    this.getRecentBoardArticleList()
  },
  methods:{
    async getRecentBoardArticleList(){
      if(process.browser){
        const data = await this.$api.$get('/main')
  
        if(data.error){
          return
        }
  
        this.mainContent = data.content.sort((a,b) => new Date(a.createdAt).getTime() > new Date(b.createdAt).getTime()) 
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.board-card-container{
  width:100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap:45px 40px;

}
</style>
