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
      <button class="bi bi-plus-square-fill" @click="redirectTo('addProduct?id=0')">aggiungi prodotto</button>
    </div>

    <div class="table">
      <table>
        <tr>
          <td v-for="(prod, i) in products" :key="prod.name">
            <div class="row">
              <div class="inbl card col">
                <div class="card-body">
                  <h5 class="card-title">{{ prod.name }}</h5>
                  <p class="card-text">Prezzo: {{ prod.price }}</p>
                  <button @click="addToCart(i)" class="btn btn-primary">
                    Aggiungi al carrello
                  </button>
                  <button @click="deleteItemFromDB(prod.id)">Delete item</button>
                  <button @click="redirectTo('addProduct?id='.concat(prod.id))">Aggiorna item</button>
                </div>
              </div>
            </div>
          </td>
        </tr>
      </table>

      <h1>Carrello:</h1>
      <div v-if="cart.length > 0">
        <span v-for="(cProd, index) in cart" :key="cProd.name">
          {{ cProd.name }} {{ cProd.price }} x {{ cProd.quantity }} =
          {{ cProd.priceCart }} €<i
            class="bi bi-trash-fill"
            @click="deleteItem(index)"
          ></i>
          <br />
        </span>
      </div>
      <button @click="getTotal()">Calcola totale</button>
      <div v-if="isTotalVisible">
        <h1>Totale: {{ total }}</h1>
      </div>
    </div>
  </div>
</template>



<script lang="ts">
import { Options, Vue } from "vue-class-component";
import axios from 'axios';

@Options({
  props: {
    msg: String,
  },
})
export default class Shopping extends Vue {
  msg!: string;

  total = 0;
  isTotalVisible = false;
  cart: any = [];
  products: any = [];


restUrl='http://localhost:9090/vuebackend/rest/product/';


 beforeCreate(){
 axios
      .get(this.restUrl.concat('findAll')).then(response => {
          console.log(response.data.body)
          this.products=response.data.body
      })
  }

  addToCart(index:any) {
    if (!this.cart.includes(this.products[index])) {
      this.cart.push(this.products[index]);
      let prodIndex = this.cart.indexOf(this.products[index]);
      this.cart[prodIndex].quantity = 1;
      let price = this.products[index].price;
      this.cart[prodIndex].priceCart = price;
    } else {
      let prodIndex = this.cart.indexOf(this.products[index]);
      this.cart[prodIndex].quantity += 1;
      let price = this.products[index].price;
      let quantity = this.cart[prodIndex].quantity;
      this.cart[prodIndex].priceCart = price * quantity;
    }
  }

  deleteItem(index:any) {
    let quantityC = this.cart[index].quantity;
    quantityC = quantityC - 1;
    this.cart[index].quantity = quantityC;

    if (this.cart[index].quantity === 0) {
      this.cart.splice(index, 1);
    } else {
      let prodPrice = this.products[index].price;
      this.cart[index].priceCart = prodPrice * quantityC;
    }
  }

  getTotal() {
    this.isTotalVisible = false;
    let total = 0;
    this.cart.forEach((element:any) => {
      total += element.priceCart;
    });
  console.log(total)
    this.total = total;
    this.isTotalVisible = true;
  }

  deleteItemFromDB(id:number){
   
    axios.delete(this.restUrl.concat('delete/').concat(id.toString())).then(response => {
          console.log(response.data.body)
         
          axios
      .get(this.restUrl.concat('findAll')).then(response => {
          console.log(response.data.body)
          this.products=response.data.body
      })
      })
  }

  redirectTo(route:any){
     window.location.href=route
  }
}
</script>



