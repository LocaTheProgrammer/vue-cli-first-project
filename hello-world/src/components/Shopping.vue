
    <style>
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.8.1/font/bootstrap-icons.css");
[v-cloak] {
  display: none;
}

body {
  background-color: bisque;
  font-family: Arial, Helvetica, sans-serif;
  font: 500;
  width: 100%;
}
.navbar {
  background-color: #a9dd6d;
  min-height: 10%;
  vertical-align: baseline;
}
.inbl {
}
.row {
  text-align: center;
}
</style>


<template>
  <div class="container">
    <div class="navbar">
      <i class="bi bi-house-fill fa-10x" @click="redirectTo('127.0.0.1')"
        >home</i
      >
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
  products: any = [
    {
      name: "banana",
      image:
        "https://upload.wikimedia.org/wikipedia/commons/8/8a/Banana-Single.jpg",
      price: 1.20,
    },
    {
      name: "carota",
      image:
        "https://d21mug5vzt7ic2.cloudfront.net/s24/86588/resize/86588_1.jpg",
      price: 1.10,
    },
    {
      name: "mela",
      image:
        "https://www.antoniopaolillo.com/wp-content/uploads/2020/03/mela-rossa-1-scaled.jpg",
      price: 1.00,
    },
    {
      name: "pomodoro",
      image:
        "https://www.finagricola.it/wp-content/uploads/2020/02/pomodoro-oblungo-rosso2-1.jpg",
      price: 0.80,
    },
    {
      name: "broccolo",
      image:
        "https://consumatori.e-coop.it/wp-content/uploads/2017/01/Broccoli-BIG.png",
      price: 1.50,
    },
    {
      name: "pesca",
      image:
        "https://www.biotipioberhammer.it/wp-content/uploads/2019/08/pesca-frutto-depurante.jpg",
      price: 1.30,
    },
  ];

  addToCart(index) {
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

  deleteItem(index) {
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
    this.cart.forEach((element) => {
      total += element.priceCart;
    });
  console.log(total)
    this.total = total;
    this.isTotalVisible = true;
  }
}
</script>



