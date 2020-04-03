<template>
    <main>
        <nav>
            <button :class="activeButtonLatest" @click="choseLatestSnippets" class="showThis" :disabled="isDisabled">Latest</button>
            <button :class="activeButtonBest" @click="choseBestSnippets" class="showThis" :disabled="isDisabled">Best</button>
            <button :class="activeButtonReported" @click="choseReportedSnippets" class="showThis reported" :disabled="isDisabled">Reported</button>
        </nav>
        <div class="loadingMessages">{{loadingMessage}}</div>
        <Snippets @updateList="updateList"
        @updateReportList="updateReportList"
        @updateLoadingDiv="updateLoadingDiv($event)"
         :snippetList="snippetList"
         :isDisabled="isDisabled"/>
    </main>
  
</template>

<script>
import Snippets from './Snippets.vue'
    export default {
        name: 'Mainbody',
        data: () => ({

        }),
        computed: {
            activeButtonLatest(){
                return this.showLatestSnippets? 'activeButton' : '';
            },
            activeButtonBest(){
                return this.showBestSnippets? 'activeButton' : '';
            },
            activeButtonReported(){
                return this.showReportedSnippets? 'activeButton' : '';
            }
      
        },
        methods:{

            choseLatestSnippets(){
                this.$emit('choseLatestSnippets');
            },
            choseBestSnippets(){
                this.$emit('choseBestSnippets');
            },
            choseReportedSnippets(){
                this.$emit('choseReportedSnippets');
            },
            updateList(){
                this.$emit('updateList');
            },
            updateReportList(){
                this.$emit('updateReportList');
            },
            updateLoadingDiv(event){
                this.$emit('updateLoadingDiv', event);
            }
        },
        components: {
            Snippets
        },
        props:{
            snippetList: Array,
            loadingMessage: String,
            isDisabled: Boolean,
            showLatestSnippets: Boolean,
            showBestSnippets: Boolean,
            showReportedSnippets: Boolean
        }

    }
</script>

<style>
    nav{
        background-color: grey;
        display: flex;
        justify-content: center;
        margin: 1rem 0;
    }

    button.showThis {
        padding: 1rem 0;
        width: 8em;
        margin: .5rem 2rem;
        border-radius: 3px;
    }
    button.activeButton{
        background-color: rgb(255, 71, 71);
        color: white;
        font-weight: 600;
    }
    div.loadingMessages{
        width: 100%;
        height: 2em;
        background-color: whitesmoke;
        margin-bottom: 1rem;
        text-align: center;
        line-height: 2em;
        color: red;
    }

</style>

