<template>
  <div class="col-md-12 my-2" style="font-family: 'Poppins', sans-serif;">
    <div class="col-12 m-0 p-0">
      <div class="d-flex col-11 p-0 m-auto justify-content-between">
        <div class="d-flex">
          <h3 style="font-weight:500">Virtual Cards </h3>
        </div>
        <div>
          <p class="ml-5"><input type="search" v-model="search" placeholder="Search"></p>
        </div>
        <div>
          <div class="dropdown">
            <a class="btn btn-secondary dropdown-toggle" href="#" role="button" id="dropdownMenuLink" data-bs-toggle="dropdown" aria-expanded="false">
              Filter
            </a>

            <div class="dropdown-menu p-1" style="text-align:center" aria-labelledby="dropdownMenuLink">
              <h5>Type</h5>
              <hr>

              <div class="d-flex justify-content-between" style="font-size:12px">
                <input type="checkbox" v-model="type" value="burner" name="" id="">Burner
                <input type="checkbox" v-model="type" value="subscription" name="" id="">Subscription
              </div>
              <div class="mt-2" >
                <p><button class="btn btn-primary">Apply</button></p>

              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row col-11 m-auto p-0" >
        <div class="col-6  " v-for="(data, index) of filteredList " :key=index>
          <div class="col-12 border m-3 p-3">
            <div class="d-flex justify-content-between">
              <div style="text-align:left">
                <h6>{{data.name}}</h6>
                <p>{{data.personal_name}} <span style="font-size:12px">software subscription</span></p>
              </div>
              <div>
                <p v-if="data.card_type == 'burner'"><i class="fa fa-fire" style="font-size:25px;color:red" aria-hidden="true"></i></p>
                <p v-if="data.card_type == 'subscription'"><i class="fa fa-check-circle" style="font-size:24px;color:green"></i></p>
              </div>
            </div>
            <div class="d-flex justify-content-between">
              <p class="border p-1" style="font-size:12px">{{data.card_type}}</p>
              <p v-if="data.card_type == 'burner'">Expires: {{data.expiry}}</p>
              <p v-if="data.card_type == 'subscription'">Limit: {{data.limit }} SGD</p>
            </div>
            <hr class="py-1" style="background:#219652">
            <div class="d-flex justify-content-between">
              <p>Spent</p>
              <p>{{data.spent[0].value}}</p>
            </div>
            <div class="d-flex justify-content-between">
              <p>Available to spend</p>
              <p>{{data.available_to_spend[0].value}}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="todos.length" v-observe-visibility="handlescrolltobottom">

    </div>
  </div>
</template>

<script>
import axios from "axios"
// const baseURL = ""
export default {
 data(){
   return{
     todos:[],
     todoName : '',
     search:null,
     type:[],
     page:1,
     last_page:1
   }
 },

  methods:{
    // async addtodo(){
    //   const res =await axios.post(, {name:this.todoName});
    //   this.todos = [...this.todos, res.data]
    //   this.todoName = ''
    // },
    async fetch(){
      const res = await axios.get(`http://localhost:3000/todos?page=${this.page}`);
      this.todos.push(...res.data)
      this.last_page = res.data.meta.last_page
    },
    handlescrolltobottom( isVisible){
      if(!isVisible){
        if(this.page>=this.last_page){
          return
        }
       return
      }
      this.page++
      this.fetch()
    }
  },
  mounted(){
    this.fetch()
  },
  computed: {
    filteredList() {
        if(this.search ){
          return this.todos.filter(todo => todo.personal_name.includes(this.search.toLowerCase()))
        }
        if(this.type){
          return this.todos.filter(todo => todo.card_type.includes(this.type))
        }
        return this.todos
    }

  }


}
</script>

<style scoped>
  .button-style{
    border:none;
    color:black
  }
</style>