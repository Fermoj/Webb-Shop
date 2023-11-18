<template>
  <v-app>
    <v-container>
      <h2 class="text-center ma-6 head-text">SHOPPING CART</h2>
      <v-row>
        <v-col v-for="(item, index) in cartItems" :key="index" cols="6" md="3">
          <v-card
            style="
              background-image: linear-gradient(
                #000000 0%,
                #000000 30%,
                #e351fc 100%
              );
            "
            class="pa-2"
          >
            <v-img src="Team3-images/CartImage/happy.png"></v-img>

            <v-card-title class="product-text">{{ item.name }}</v-card-title>
            <v-card-text class="size-text mt-4">
              <div class="mb-3">Antal: {{ item.quantity }}st</div>
              <div class="mb-3">Size: {{ item.size }}</div>
              <div>Price: {{ item.price }} Kr</div>
            </v-card-text>
            <v-card-actions>
              <v-btn
                elevation="24"
                class="ma-2"
                color="purple-darken-4"
                @click="removeItem(index)"
                >Remove <v-icon end icon="mdi mdi-delete"></v-icon
              ></v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <v-row class="d-flex justify-center">
        <v-col>
          <div class="d-flex justify-center mt-1">
            <v-card width="360px">
              <v-card-title class="text-center payment-text">
                Shipping
              </v-card-title>
              <v-card-text>
                <v-text-field label="Name" />
                <v-text-field label="Street" />
                <v-text-field label="Zip" />
                <v-text-field label="City" />
                <v-text-field label="Phone" />
                <v-text-field label="Email" />
              </v-card-text>
            </v-card>
          </div>
        </v-col>

        <v-col>
          <div class="d-flex justify-center">
            <v-card width="360px" outlined>
              <v-card-title class="text-center payment-text">
                Payment
              </v-card-title>
              <v-radio-group>
                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox
                    value="1"
                    color="pink-lighten-3"
                    label="Klarna"
                  ></v-checkbox>
                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/klarna.png"
                  ></v-avatar>
                </v-card-text>

                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox
                    value="2"
                    color="orange"
                    label="Visa"
                  ></v-checkbox>
                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/VisaL.png"
                  ></v-avatar>
                </v-card-text>
                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox value="3" label="Swish"></v-checkbox>
                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/swish.jpg"
                  ></v-avatar>
                </v-card-text>
              </v-radio-group>
            </v-card>
          </div>
        </v-col>
        <v-col>
          <div class="d-flex justify-center">
            <v-card width="360px" outlined>
              <v-card-title class="text-center payment-text">
                Delivery Options
              </v-card-title>
              <v-radio-group>
                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox
                    value="1"
                    color="teal-accent-3"
                    label="Budbee"
                  ></v-checkbox>
                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/Bud.png"
                  ></v-avatar>
                </v-card-text>
                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox
                    value="2"
                    color="cyan-accent-4"
                    label="Post Nord"
                  ></v-checkbox>

                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/post.png"
                  ></v-avatar>
                </v-card-text>
                <v-card-text class="d-flex text-center mt-11">
                  <v-checkbox
                    value="3"
                    color="yellow-accent-4"
                    label="DHL"
                  ></v-checkbox>
                  <v-avatar
                    size="x-large"
                    image="Team3-images/CartImage/dhl.png"
                  ></v-avatar>
                </v-card-text>
              </v-radio-group>
            </v-card>
          </div>
        </v-col>
      </v-row>

      <div>
        <v-card-text class="d-flex">
          <v-text-field label="* GiftCard" />
          <v-spacer />
          <v-text-field label="* Discount Code" />
        </v-card-text>
        <div class="d-flex">
          <v-checkbox>
            <template v-slot:label>
              <div>
                agree to our
                <v-tooltip location="top">
                  <template v-slot:activator="{ props }">
                    <a
                      target="_blank"
                      href="https://iths.se"
                      v-bind="props"
                      @click.stop
                    >
                      terms
                    </a>
                  </template>
                  Opens in new window
                </v-tooltip>
              </div>
            </template>
          </v-checkbox>
          <v-spacer />
          <v-checkbox
            class="justify-end d-flex"
            label="Subscribe for news"
          ></v-checkbox>
        </div>
      </div>
      <h3 class="d-flex justify-center mt-4 amount-text">
        Total amount: {{ totalAmount }} SEK
      </h3>
      <div class="d-flex justify-space-between">
        <v-card-actions>
          <v-btn to="/" color="purple-darken-2" variant="outlined">
            <v-icon class="mx-2" small left>mdi-arrow-left-circle</v-icon>
            Take me home
          </v-btn>
        </v-card-actions>
        <v-col cols="auto">
          <v-dialog width="auto">
            <template v-slot:activator="{ props }">
              <v-card-actions>
                <v-btn color="purple-darken-2" variant="outlined" v-bind="props"
                  ><v-icon class="mx-2" small>mdi mdi-cart-outline</v-icon> Take
                  my money</v-btn
                >
              </v-card-actions>
            </template>
            <template v-slot:default="{ isActive }">
              <v-card>
                <v-toolbar
                  class="text-center"
                  color="purple-darken-1"
                  title="HELLO BUDDY!!"
                ></v-toolbar>
                <v-card-text>
                  <div class="text-h2 pa-12 text-center">
                    Thank you for your purchase
                  </div>
                  <div class="text-h6 pa-12 text-center">
                    We will thank you with a 15% discount code for your next
                    purchase!
                    <p>Use <span>PARTYFEET15</span> for 15% discount</p>
                  </div>
                </v-card-text>
                <v-card-actions class="justify-end">
                  <v-btn to="/" variant="text" @click="isActive.value = false"
                    >To homepage</v-btn
                  >
                </v-card-actions>
              </v-card>
            </template>
          </v-dialog>
        </v-col>
      </div>
    </v-container>
  </v-app>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return { name: "", price: "", socks: [], cartItems: [], checkbox: false };
  },
  created() {
    axios
      .get("/Product.json")
      .then(response => {
        this.products = response.data;
      })
      .catch(error => {
        console.log(error);
      });
    if (localStorage.getItem("cartItems")) {
      this.cartItems = JSON.parse(localStorage.getItem("cartItems"));
    }
  },
  computed: {
    totalAmount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.price * item.quantity;
      }, 0);
    }
  },
  methods: {
    addToCart(sock) {
      const existingItem = this.cartItems.find(item => item.name === sock.name);
      if (existingItem) {
        existingItem.quantity++;
      } else {
        this.cartItems.push({
          name: sock.name,
          price: sock.price,
          quantity: 1
        });
      }
      localStorage.setItem("cartItems", JSON.stringify(this.cartItems));
    },
    removeItem(index) {
      this.cartItems.splice(index, 1);
      localStorage.setItem("cartItems", JSON.stringify(this.cartItems));
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Barrio&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Gruppo&display=swap");
.head-text {
  font-family: "Barrio", cursive;
  letter-spacing: 2rem;
  background-image: linear-gradient(to right, #00dbde 0%, #fc00ff 100%);
  font-size: 3em;
  background-size: 9%;
  background-repeat: repeat;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-background-clip: text;
  -moz-text-fill-color: transparent;
}
.product-text {
  font-size: 22px;
  color: rgb(255, 255, 255);
  font-weight: bolder;
  font-family: "Gruppo", cursive;
}

.size-text {
  font-size: 22px;
  color: rgb(0, 0, 0);
  font-weight: 600;
  font-family: "Gruppo", cursive;
}

.amount-text {
  font-size: 22px;

  font-weight: 600;
  letter-spacing: 0.1rem;
  font-family: "Gruppo", cursive;
  font-size: 1.8em;
}

.payment-text {
  font-size: 35px;

  font-weight: bolder;
  font-family: "Gruppo", cursive;
}
span {
  color: #ae03b1;
}
</style>