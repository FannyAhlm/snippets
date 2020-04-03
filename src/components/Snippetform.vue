<template>
<div class="formComponent">
  <div class="form">
    <div class="title">
      <label for="snippetTitle">Title:</label>
      <input v-model="title" type="text"  :class="titleInvalid" id="snippetTitle" placeholder="Snippets title here">
    </div>
    <div class="content">
      <label for="snippetContent" >Content:</label>
      <textarea placeholder="Your code snippet goes here" v-model="content" :class="contentInvalid" id="snippetContent" cols="50" rows="6"></textarea>
      <div class="errorDiv" :class="hideErrorDiv" >Error: each field requires a minimum of two charachters.</div>
    </div>
     <button :disabled="isDisabled" @click="validForm" :class="disabledClass">Create snippet!</button>
  </div>
</div>
</template>

<script>
export default {
  name: 'Snippetform',
  data: () => ({
    title: '',
    content: '',
    baseUrl: 'https://www.forverkliga.se/JavaScript/api/api-snippets.php',
    titleValid: true,
    contentValid: true,
    error: false
  
  }),
  computed:{
    disabledClass(){
      return this.isDisabled ? "disabled" : ""
    },
    titleInvalid(){
      return this.titleValid ? "" : "invalid";
    },
    contentInvalid(){
      return this.contentValid ? "" : "invalid";
    },
    hideErrorDiv(){
      return this.error ? "" : "hide"
    }
  },

  methods:{
    validForm(){
      if(this.title.length > 1 && this.content.length > 1){
        this.titleValid = true;
        this.contentValid = true;
        this.error = false;
        this.postSnippet();
      } else if(this.title.length < 2 && this.content.length < 2) {
        this.titleValid = false,
        this.contentValid = false,
        this.error = true;
      } else if( this.title.length < 2){
        this.titleValid = false,
        this.error = true;
      } else {
        this.contentValid = false,
        this.error = true;
      }
    },

    async postSnippet(){
      this.$emit('updateLoadingDiv', 'Sending snippet');
      try{
        let response = await this.$http.post(this.baseUrl,
        { add: '', title: this.title, content: this.content
        });
        if(response.data.status == "success"){
          this.$emit('updateList');
        }
      }
      catch{
        console.log('det här funkar fan inte')
      }
      finally{
        this.title = '';
        this.content = '';
      }
    },
  },
  props:{
    snippetList: Array,
    isDisabled: Boolean
  }
}

</script>

<style scoped>
.formComponent{
  height: 15rem;
  background-color: white;
}
  div.form{
    position: relative;
    height: 100%;
    width: 30rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
  }
  label{
    position: absolute;
    right: 25.5rem;
  }
  button{
    background-color: rgb(230, 68, 68);
    font-size: 1.1rem;
    color: white;
    border: 1px solid black;
    text-transform: uppercase;
    font-family: monospace;
  }
  textarea{
    resize: none;
  }
  div.errorDiv{
    font-size: .8rem;
    color: red;
  }
  div.hide{
    opacity: 0;
  }

  input,
  button,
  textarea,
  div.errorDiv{
    width: 20rem;
  }
  input,
  textarea{
    padding: .3rem;
    border: 1px solid grey;
  }
  div.title,
  div.errorDiv{
    margin-bottom: .5rem;
  }
  :placeholder-shown{
    opacity: 70%;
  }
  div.form,
  button{
    padding: 1em;
  }
  button.disabled{
    opacity: 50%;
  }
  .hide{
    opacity: 0;
  }
  .invalid{
    border: 1px solid red;
  }
</style>
