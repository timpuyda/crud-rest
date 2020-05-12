<template>
  <div id="app">
    <input type="text" v-model="inputText">
    <button @click = "postFunc">post</button>
    <table>
      <author
       v-for="post in posts" 
       :key=post.id
       :post="post"
       @delete-item="delFunc"
       @edit-item="putFunc"
      ></author>
    </table>
  </div>
</template>

<script>
import Author from './components/Author.vue'

console.clear()
export default {
  name: 'App',
  components:{
    author: Author
  },
  data(){
    return{
      posts:[],
      url: 'http://localhost:3000',
      inputText: '',
    }
  },
  methods:{
    postFunc() {
      if (!this.inputText.replace(/^\s*/,'').replace(/\s*$/,'')) return
      this.$axios
      .post(this.url + '/posts/', {
       author: this.inputText,
     })
      .then(()=> {
        this.getFunc();
        this.inputText= '';
      })
        
      .catch(function (error) {
       console.log(error);
     })
    },
    delFunc(id){
      this.$axios
      .delete(this.url + '/posts/'+id)
      .then(()=> this.getFunc())
      .catch(er => console.log(er))
    },
    getFunc(){
      this.$axios
      .get(this.url+ '/posts')
       .then( response => {
       this.posts = response.data
       console.log(this.posts)
      })
      .catch( error =>console.log(error))
    },
    putFunc(itemData){
      if (!itemData.editText.replace(/^\s*/,'').replace(/\s*$/,'')) return
      this.$axios
      .put(this.url + '/posts/' + itemData.id,{
        author: itemData.editText
      })
      .then(()=> {
        this.getFunc()
      })
      .catch(er=>console.log(er))
    }
  },
  mounted(){
    this.getFunc()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table{
  border-collapse: collapse;
  text-align: justify;
}
td{
  padding: 0.3rem;
}
tr{
  border:1px solid #444;
}
</style>
