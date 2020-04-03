<template>
  <div id="app">
    <h1>Share you snippets!</h1>

    <Snippetform @updateList="getLatestSnippets"
    @updateLoadingDiv="pageLoading($event)"
     :snippetList="snippetList"
     :isDisabled="isDisabled"/>

    <Mainbody 
    @updateList="getLatestSnippets"
    @updateLoadingDiv="pageLoading($event)" 
    @updateReportList="getReportedSnippets"
    @choseLatestSnippets="getLatestSnippets" @choseBestSnippets="getBestSnippets" @choseReportedSnippets="getReportedSnippets" :snippetList="snippetList"
    :loadingMessage="loadingMessage"
    :isDisabled="isDisabled"
    :showBestSnippets="showBestSnippets"
    :showLatestSnippets="showLatestSnippets"
    :showReportedSnippets="showReportedSnippets"/>
  </div>
</template>

<script>
import Snippetform from './components/Snippetform.vue'
import Mainbody from './components/Mainbody.vue'

export default {
  name: 'App',
  components: {
    Snippetform,
    Mainbody
  },
  data: () => ({
    snippetList: [],
    baseUrl: 'https://www.forverkliga.se/JavaScript/api/api-snippets.php',
    loadingMessage: '',
    isDisabled: false,
    showLatestSnippets: true,
    showBestSnippets: false,
    showReportedSnippets: false
  }),
  created(){
    this.getLatestSnippets();
  },
  methods: {

    pageLoading(event){
      this.loadingMessage = event;
      this.isDisabled = true;
    },
    async getLatestSnippets(){
      try{
        this.loadingMessage = "Getting latest snippets";
        let response = await this.$http.get(this.baseUrl, {params: {latest: ''}});
        this.snippetList = response.data;

      }
      catch(error){
        this.loadingMessage = 'Something went wrong, please try again';
      }
      finally{
        this.loadingMessage = '';
        this.isDisabled = false;
        this.showLatestSnippets = true,
        this.showBestSnippets = false,
        this.showReportedSnippets = false
      }
    },
        
    async getBestSnippets(){
      this.isDisabled = true;
        try{
            this.loadingMessage = "Getting best scored snippets";
            let response = await this.$http.get(this.baseUrl, {params: {best: ''}});
            this.snippetList = response.data;
            console.log('getBestSnippets körs');
        }
        catch(error){
          this.loadingMessage = 'Something went wrong, please try again';
        }
        finally{
          this.loadingMessage = '';
          this.isDisabled = false;
          this.showLatestSnippets = false,
          this.showBestSnippets = true,
          this.showReportedSnippets = false
        }
    },
    async getReportedSnippets(){
      this.isDisabled = true;
      try{
        this.loadingMessage = "Getting reported snippets";
        let response = await this.$http.get(this.baseUrl, {params: {reported: ''}});
        this.snippetList = response.data;
      }
      catch(error){
        this.loadingMessage = 'Something went wrong, please try again';
      }
      finally{
        this.loadingMessage = '';
        this.isDisabled = false;
        this.showLatestSnippets = false,
        this.showBestSnippets = false,
        this.showReportedSnippets = true
      }
    },
  }
}

</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    min-height: 100vh;
  }
  body{
    background-color: rgb(189, 189, 189);
  }
  h1{
    margin: 0 auto;
    padding: 1rem 0;
    width: 298px;
  }
  button{
    cursor: pointer;
  }
</style>
