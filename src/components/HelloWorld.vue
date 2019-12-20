<template>
  <div class="hello">
    <div>
      <label for="">firstname
        <input type="text" v-model="firstname">
      </label>
      <label for="">lastname
        <input type="text" v-model="lastname">
      </label>
      <button @click="add">Add</button>
    </div>
    <ul>
      <li v-for="(user,i) in users" :key="i">
        {{user.firstname}}  {{user.lastname}} <button  @click="detail(user.id)">Seleziona</button>
        </li>
    </ul>
    {{user}}
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'HelloWorld',
  data(){
    return{
      users:[],
      user:{},
      firstname:'',
      lastname:''
    }
  },
  async mounted(){
    try{
     let res = await axios({
        method: "POST",
        url:"http://127.0.0.1:3333/graphql",
        data:{
         query: `
          query allUsers {
            allUsers {
              id
              firstname,
              lastname
              }
            }
          `
        }
      })
    this.users= res.data.data.allUsers

    }catch(e) {
      console.log(e)
    }
  },
  methods:{
    async detail(id){
      try{
     let res = await axios({
        method: "POST",
        url:"http://127.0.0.1:3333/graphql",
        data:{
         query: `
          query fetchUser {
            fetchUser(id:${id}){
              id
              firstname,
              lastname,
              job{
                description
                }
              }
            }
          `
        }
      })
     console.log(res.data.data.fetchUser)
    this.user= res.data.data.fetchUser

    }catch(e) {
      console.log(e)
    }
    },

 async add(){
    try{
      let res = await axios({
         method: "POST",
        url:"http://127.0.0.1:3333/graphql",
        data:{
         query: `
          mutation {
          createUser(firstname:"${this.firstname}",lastname:"${this.lastname}"){
          firstname
          lastname
          }
        } 
         `
        }
      })
     console.log(res)

    }catch(e){
      console.log(e)
    }

  }


  }
 

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
