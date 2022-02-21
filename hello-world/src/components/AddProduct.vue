<style>
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css");
[v-cloak] {
  display: none;
}

.container {
  background-color: rgb(196, 255, 204);
  font-family: Arial, Helvetica, sans-serif;
  font: 500;
  width: 100%;
}
.navbar {
  background-color: #a9dd6d;
  min-height: 10%;
  vertical-align: baseline;
}
.row {
  text-align: center;
}
</style>


<template>
   <div class="container">
    <div class="navbar">
      <button class="bi bi-house-fill fa-10x" @click="redirectTo('127.0.0.1')"
        >home</button
      >
      <button class="bi bi-plus-square-fill" @click="redirectTo('127.0.0.1')">aggiungi prodotto</button>
    </div>

    <div class="form">
        Nome: <input type="text" v-model="name">
       
        Prezzo: <input type="number" v-model="price">
        <button @click="send()">INVIA</button>
    </div>
   
  </div>
</template>



<script lang="ts">
import { Options, Vue } from "vue-class-component";
import axios from 'axios';
import { useRoute } from 'vue-router';

@Options({
  props: {
    msg: String,
  },
})
export default class AddProduct extends Vue {
  msg!: string;
  name=""
  price=0
  product:Product
  prodId:any
  restUrl='http://localhost:9090/vuebackend/rest/product/';

    beforeCreate(){
     const route = useRoute();
    this.prodId=route.query.id
    if(this.prodId!==0){
        axios.get(this.restUrl.concat('findById/').concat(this.prodId)).then(response=>{
            this.name=response.data.body.name
            this.price=response.data.body.price
        })
    }
    }

    send(){
       
        this.product={
            id:this.prodId,
            name:this.name,
            price:this.price
        }
        axios
      .post(this.restUrl.concat('create'), this.product).then(response => {
          console.log(response.data.body)
         
      })
    }


}
export class Product{
    id:number
    name:string
    price:number
}
</script>



