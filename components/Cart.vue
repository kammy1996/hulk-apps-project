<template>
  <div>
    <b-sidebar
      v-model="isSidebar"
      id="sidebar-right"
      right
      width="450px"
      shadow
    >
      <div class="cart-area">
        <div class="cart-items">
          <p class="cart-title">
            <span class="cart-heading">Your Bag </span>
            <span class="cart-items">({{ cartCount || 0 }} Items)</span>
          </p>

          <div
            class="empty-bag text-center"
            v-if="products && products.length <= 0"
          >
            <div class="height-20"></div>
            <b-img
              width="100%"
              src="../static/assets/images/empty-cart.png"
            ></b-img>
            <div class="height-20"></div>
            Your Bag is Empty
          </div>
          <div
            v-else
            class="product-preview mt-3"
            v-for="(product,index) in products"
            :key="index"
          >
            <b-row>
              <b-col cols="3">
                <b-img
                  class="preview-img"
                  width="100%"
                  :src="product.current_image"
                ></b-img>
              </b-col>
              <b-col cols="6">
                <div class="product-preview-info ml-2">
                  <span class="preview-title">{{ product.title }}</span
                  ><br />
                  <span class="preview-color"
                    >Color: {{ product.selected_color }} </span
                  ><br />
                  <span class="preview-size"
                    >Size: {{ product.selected_size }} </span
                  ><br />
                  <div class="height-10"></div>
                  <b-input-group size="sm" class="quantity-input">
                    <b-input-group-prepend>
                      <b-btn
                        class="quantity-btn mr-n2"
                        @click="changeQuantity(product.id, 'minus')"
                        >-</b-btn
                      >
                    </b-input-group-prepend>

                    <b-form-input
                      class="input-box"
                      type="number"
                      min="0.00"
                      v-model="product.quantity"
                    ></b-form-input>

                    <b-input-group-append>
                      <b-btn
                        class="quantity-btn ml-n3"
                        @click="changeQuantity(product.id, 'plus')"
                        >+</b-btn
                      >
                    </b-input-group-append>
                  </b-input-group>
                </div>
              </b-col>
              <b-col cols="3">
                <div class="text-right">
                  <p>
                    <b-icon
                      icon="trash"
                      class="cursor-pointer remove-icon"
                      scale="1"
                      @click="removeFromCart(index)"
                      >X</b-icon
                    >
                  </p>
                  <div class="height-50"></div>
                  <p class="preview-price">$ {{ product.product_total }}</p>
                </div>
              </b-col>
            </b-row>
          </div>
          <div class="height-20"></div>
          
        </div>
        <div class="cart-footer">
            <b-row>
              <b-col cols="8">
                <p class="cart-footer-title">SubTotal</p>
              </b-col>
              <b-col cols="4" class="text-right">
                <p class="cart-footer-total">${{ subTotal }}</p>
              </b-col>
            </b-row>
            <button class="checkout-btn hvr-sweep-to-right">
              <b-icon icon="check2" scale="1.2" class="mr-1"></b-icon> Checkout
            </button>
            <p class="mt-2 text-center promo-code-text">
              Have a promocode ? Enter your code at checkout. Shipping & Taxes
              are calculated during checkout.
            </p>
          </div>
      </div>
    </b-sidebar>
    <Alert :isAlert="alert.isAlert" :variant="alert.variant" :msg="alert.msg" />
  </div>
</template>

<script>
import _ from "lodash";
import Alert from "@/components/common/Alert";

export default {
  name: "Cart",
  data() {
    return {
      isSidebar: false,
      quantity: 1,
      products: [],
      cartCount: 0,
      subTotal: 0,
      alert: {},
    };
  },
  created() {
    this.$nuxt.$on("toggle-cart", () => {
      this.isSidebar = true;
    });
    this.$nuxt.$on("added-to-cart", () => {
      this.getCartProducts();
      this.isSidebar = true;
    });
  },
  watch: {
    products: {
      deep: true,
      handler(val) {
        this.calcSubTotal();
      },
    },
  },
  mounted() {
    this.getCartProducts();
  },
  components: {
    Alert,
  },
  methods: {
    calcSubTotal() {
      let allTotals = this.products.map((product) =>
        Number(product.product_total)
      );
      if (allTotals && allTotals.length > 0) {
        this.subTotal = allTotals.reduce((p, c) => p + c);
      } else {
        this.subTotal = 0;
      }
    },
    changeQuantity(product_id, type) {
      let index = this.products.findIndex(
        (product) => product.id == product_id
      );

      if (type == "minus") {
        if (this.products[index].quantity <= 1) {
          this.alert = {
            isAlert: true,
            variant: "danger",
            msg: `Quantity of the product cannot be negative.`,
          };
          setTimeout(() => (this.alert = {}), 2000);
          return;
        }
      }

      if (type == "plus") this.products[index].quantity++;
      else this.products[index].quantity--;

      this.calcPriceAsPerQuantity(this.products[index]);
    },
    calcPriceAsPerQuantity(product) {
      product.product_total = product.current_price * product.quantity;
    },
    getCartProducts() {
      let raw = window.localStorage.getItem("cart");
      if (raw) {
        this.products = JSON.parse(raw);
        this.cartCount = this.products.length;
      }
    },
    removeFromCart(index) {      
      this.alert = {
        isAlert: true,
        variant: "success",
        msg: `${this.products[index].title} has been removed from cart.`,
      };
      setTimeout(() => (this.alert = {}), 2000);
      this.products.splice(index, 1);
      this.cartCount = this.products.length;

      let stringed = JSON.stringify(this.products);
      window.localStorage.setItem("cart", stringed);
      $nuxt.$emit("removed-from-cart");
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../static/assets/hover.css";

.cart-area {
  padding: 10px 15px;

  .product-preview {
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    padding-bottom: 15px;
    margin: 0px 10px;
  }

  .cart-heading {
    font-size: 20px;
    font-weight: 600;
    text-transform: uppercase;
  }

  .preview-title {
    font-size: 14px;
    text-transform: uppercase;
    font-weight: 600;
  }

  .preview-color,
  .preview-size {
    font-size: 13px;
  }

  .preview-price {
    font-weight: 600;
  }

  .quantity-btn {
    padding: 0px 10px;
    background: rgba(0, 0, 0, 0.1);
    border: none;
    border-radius: 100px;
    color: black;
    font-weight: 500;
    box-shadow: none !important;
  }

  .quantity-input {
    width: 110px;
    input[type="number"]::-webkit-inner-spin-button,
    input[type="number"]::-webkit-outer-spin-button {
      -webkit-appearance: none;
    }

    .input-box {
      background: transparent;
      border: none;
      padding: 0px 30px;
    }

    .input-box:focus {
      box-shadow: none;
    }
  }

  .cart-items {
    height:480px !important;
    overflow-y:scroll;
    overflow-x:hidden
  }

  .cart-footer {
    border-top: 1px solid rgba(0, 0, 0, 0.2);
    bottom: 0;
    padding: 20px;
    position:fixed;

    .cart-footer-title,
    .cart-footer-total {
      text-transform: uppercase;
      font-weight: 700;
    }

    .checkout-btn {
      background: transparent;
      border: 1px solid black;
      color: black;
      border-radius: 0px;
      font-size: 15px;
      text-align: center;
      display: block;
      width: 100%;
      text-transform: uppercase;
      box-shadow: none;
      padding: 8px 0px;
    }

    .promo-code-text {
      font-size: 13px;
    }
  }
}
</style>
