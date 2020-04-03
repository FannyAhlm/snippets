<template>
    <div class="snippetGrid">
        <div class="card" v-for="snippet in snippetList" :key="snippet.id">
            <div class="text">
                <p class="title">{{snippet.title}}</p>
                <div class="content">
                    <p class="content">{{snippet.content}}</p>
                </div>
            </div>
            <div class="options">
                <button v-if="!snippet.is_reported" :disabled="isDisabled" :class="disabledClass"  class="report" @click="reportSnippet(snippet.id)">Report snippet</button>
                <button v-else class="unreport" :disabled="isDisabled" :class="disabledClass" @click="unreportSnippet(snippet.id)">Unreport snippet</button>
                <div class="vote">
                    <span>Vote:</span>
                    <button :disabled="isDisabled" @click="upvoteSnippet(snippet.id)" class="vote">+1</button>
                    <button :disabled="isDisabled" @click="downvoteSnippet(snippet.id)" class="vote">-1</button>
                </div>
                <button :class="disabledClass" :disabled="isDisabled" @click="deleteSnippet(snippet.id)" class="delete">Delete snippet</button>
            </div>
        </div>
    </div>
    
</template>

<script>
    export default {
        name: 'Snippets',
        data: () => ({
              baseUrl: 'https://www.forverkliga.se/JavaScript/api/api-snippets.php'
        }),
        computed: {
            disabledClass(){
                return this.isDisabled ? "disabled" : ""
            }
        },
        methods:{
            async deleteSnippet(id){
                this.$emit('updateLoadingDiv', 'Please wait while we delete this snippet');
                try{
                    let response = await this.$http.post(this.baseUrl, 
                        { 
                            delete: '',
                            id: id 
                        }
                    );
                    if(response.data.status == "success"){
                        this.$emit('updateList');
                    }
                    
                }
                catch(error) {
                    console.log("nu jävlar", error);

                }
                
            },
            async reportSnippet(id){
                this.$emit('updateLoadingDiv', 'Please wait while we report this snippet');
                try{
                    let response = await this.$http.post(this.baseUrl, 
                        { 
                            report: '',
                            id: id 
                        }
                    );
                    console.log(response.data, id);
                    if(response.data.status == "success"){
                        this.$emit('updateList');
                    }
                    
                }
                catch(error) {
                    console.log(error);

                }
                
            },
            async unreportSnippet(id){
                this.$emit('updateLoadingDiv', 'Please wait while we unreport this snippet');
                try{
                    let response = await this.$http.post(this.baseUrl, 
                        { 
                            unreport: '',
                            id: id 
                        }
                    );
                    if(response.data.status == "success"){
                        this.$emit('updateReportList');
                    }
                    
                }
                catch(error) {
                    console.log(error);

                }
                
            },
            async upvoteSnippet(id){
                this.$emit('updateLoadingDiv', 'Please wait while we upvote this snippet');
                try{
                    let response = await this.$http.post(this.baseUrl, 
                        { 
                            upvote: '',
                            id: id
                        }
                    );
                    if(response.data.status == "success"){
                        this.$emit('updateList');
                    }
                    
                }
                catch(error) {
                    console.log(error);

                }
                
            },
            async downvoteSnippet(id){
                this.$emit('updateLoadingDiv', 'Please wait while we downvote this snippet');
                try{
                    let response = await this.$http.post(this.baseUrl, 
                        { 
                            downvote: '',
                            id: id
                        }
                    );
                    if(response.data.status == "success"){
                        this.$emit('updateList');
                    }
                    
                }
                catch(error) {
                    console.log(error);

                }
                
            }
        },
        props: {
            snippetList: Array,
            isDisabled: Boolean
        }
    }

</script>

<style scoped>
    div.snippetGrid{
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
        width: 90vw;
        margin: 0 auto;
    }
    div.card{
        width: 30em;
        height: 10em;
        background-color: rgb(243, 243, 243);
        margin-bottom: 1em;
        border: 2px solid grey;
        display: flex;
        position: relative;
    }
    div.text{
        width: 20em;
        margin-left: 1em;
    }
    div.content{
        max-width: 20em;
        background-color: white;
        height: 5em;
        overflow: scroll;
        margin-top: 1rem;
    }
    div.options{
        padding: 1rem;
        width: 9em;
        position: absolute;
        right: 0;
    }
    div.options button.report,
    div.options button.unreport,
    div.options button.delete{
        padding: .3rem;
        font-weight: 600;
        color: red;
        cursor: pointer;
        width: 10em;
        border: 1px solid black;
    }
    div.options button.unreport{
        background-color: white;
        border: 1px solid black;
    }

    div.options button.delete{
        background-color: rgb(230, 68, 68);
        color: white;
        font-weight: 900;
    }

    div.vote{
        margin: .5rem 0 2rem;
    }
    button.vote{
        padding: .3rem;
        margin-left: .4rem;
        font-weight: 700;
        border: 1px solid grey;
        background-color: white;
    }
    span{
        font-weight: 600;
    }
    p.title{
        margin-top: 1rem;
    }
    p{
        margin: .2em 0 0;
        background-color: white;
        padding-left: 1em;
    }
    button.disabled{
        opacity: 50%;
    }

</style>