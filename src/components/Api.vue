<template>
  
    <div >
    
      <form @submit.prevent="save">
        <input type="hidden" v-model="form.id" name="" value="">
        <input type="text" v-model="form.name" name="" value=""><br/>
        <button type="submit" v-show="!updateSubmit"  name="button">submit</button>
        <button type="submit" v-show="updateSubmit" v-on:click='update(form)'  name="button">update</button>
      </form>
      
      <!---
      <h1>Todo List</h1>
       <ul v-for="item in list" :key="item.id">
         <li>{{item.name}} <br/>
         <button type="button" v-on:click="edit(item)">Edit</button> ||
          <button type="button" v-on:click="Delete(item)" >delete</button>

         </li>

       </ul>
       -->
       <div class="containerbox">
         <h1>Todo List</h1>
         <table>
           <tr>
              <th>ID</th>
              <th>Task</th>
              <th>Action</th>
           </tr>
           <tr v-for="item in list" :key="item.id">
             <td>{{item.id}}</td>
             <td>{{item.name}}</td>
             <td>
                 <button type="button" v-on:click="edit(item)">Edit</button> 
                  <button type="button" v-on:click="Delete(item)" >delete</button>
             </td>

           </tr>
         </table>

       </div>
     
    </div>

</template>

<script>
// eslint-disable-next-line no-unused-vars
import Vue from "vue";
// eslint-disable-next-line no-unused-vars
import VueAxios from "vue-axios";
import axios from "axios";

export default {
  name: "Api",

  data() {
    return {
      form:{
        id:'',
        name:''
      },
      list: [],
      updateSubmit :false
    }
  },
  methods: {
   
    load() {
      axios.get("/todos").then((res) => {
        this.list = res.data;
        //console.log(res.data);
      }).catch((err)=>{
        console.log(err)
      })
    },
    save(){
      axios.post('/todos', this.form)
      .then(()=>{
        this.load()
        this.form.name= ''
        alert('saving')
      }).catch(()=> {
        alert('saving ')
      })
    },
    edit(item){
      this.updateSubmit = true
      this.form.id = item.id
      this.form.name = item.name
    },
    
    update(form){
       axios.put("/todos/" + form.id , {
         name: this.form.name
       })
       .then(()=> {
         this.load()
         this.form.name=''
         this.updateSubmit = false
         alert('updated')
       }).catch((err)=> {
         console.log(err)
       })
    },
    Delete(item) {
         axios.delete("/todos/" + item.id)
       .then(()=> {
         this.load()
         alert('deleted')
       }).catch((err)=> {
         console.log(err)
       })
     
    }

  },
  mounted() {
    this.load();
  },
};
</script>

<style>
.containerbox {
  text-align: center;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  margin: 0 auto;
  width: 800px;
}
td,
th {
  text-align: left;
  padding: 8px;
  border: 1px solid #c39c9c;
}
tr:nth-child(even) {
  background-color: #dddddd;
}
ul{
  list-style-type: none;
}
</style>
