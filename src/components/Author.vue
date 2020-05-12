<template>
  <tr>
        <td>
          <span v-if="!isEdit">{{post.author}} / <small>id:{{post.id}}</small></span>
           <input type="text" v-if="isEdit" v-model="editText" >
         </td>
        <td><button @click = "editItem(post.id); isEdit = !isEdit">edit</button></td>
        <td><button @click = "deleteItem(post.id)" style="color: red">delete</button></td>
      </tr>
</template>

<script>
export default {
  name: 'Author',
  props:["post"],
  data(){
    return{
      isEdit: false,
      editText: '', 
    }
  },
  methods:{
    deleteItem(item){
      this.$emit('delete-item',item)
    },
    editItem(item){
      this.$emit('edit-item', 
        {
          id:item,
          editText:this.editText,
      })
    this.editText = '';  
    }
  }
}
</script>

