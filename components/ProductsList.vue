<template>
  <div>
    <b-container>
      <b-row >
        <b-col lg="3" class="mb-4"  v-for="product in productsList" :key="product.id">
          <div class="product-box">
            <!-- figure out a way that you can run the carousel only when you hover on it -->
            <img
              width="100%"
              class="product-image"
              :src="product.current_image"
              alt=""
            />

            <div class="height-10"></div>
            <b-row no-gutters class="mb-n2">
              <b-col cols="8">
                <p class="product-title">
                  {{ product.title }}
                </p>
              </b-col>
              <b-col cols="4">
                <p class="product-price">$ {{ product.current_price }}</p>
              </b-col>
            </b-row>
            <div class="product-colors mb-3">
              <span
                :style="[product.selected_color == color ? setActive(color) : {background: color}]"
                v-for="color in product.options[0].values"
                :key="color.id"
                @click="attachColor(product.id, color)"
              ></span>
            </div>
          </div>
          <div class="product-sizes mb-3" v-if="product.options[1]">
            <span
              class="cursor-pointer"
              :class="product.selected_size == size ? 'size-active' : ''"
              v-for="(size, index) in product.options[1] &&
              product.options[1].values"
              :key="index"
              @click="attachSize(product.id, size)"
              >{{ size }}
            </span>
          </div>
          <div v-else class="mb-3" >Size not available</div>
          <b-button class="mb-2 add-to-bag" @click="addToCart(product)"
            ><b-icon class="mr-2" icon="bag" scale="0.8"></b-icon>Add to
            Bag</b-button
          >
        </b-col>
      </b-row>  

    </b-container>
    <Alert :isAlert="alert.isAlert" :variant="alert.variant" :msg="alert.msg" />
  </div>
</template>

<script>
import ProductsList from "@/models/productsList";
import Alert from "@/components/common/Alert";
import _ from "lodash";

export default {
  name: "ProductsList",
  data() {
    return {
      productsList: ProductsList,
      cart: [],
      alert: {},
    };
  },
  components: {
    Alert,
  },
  mounted() {
    this.setInitImageAndPrice();
  },
  computed: {},
  methods: {
    setActive(color) { 
      return { 
        border:`4px solid ${color}`,
        'outline-style': 'solid',
        'outline-width': '1px',
        'outline-color':'black'
      }
    },
    showImageAsPerColor() {},
    addToCart(product) {
      let isColorSizeTaken = this.isColorSizeTaken(product);
      if(!isColorSizeTaken) { 
        this.alert = {
          isAlert: true,
          variant: "warning",
          msg: `Please Select Color and Size of ${product.title}`,
        };
        setTimeout(() => (this.alert = {}), 2000);
        return;
      }
      let raw = window.localStorage.getItem("cart");
      let parsed;
      if (raw) {
        parsed = JSON.parse(raw);
        this.cart = parsed;

        let sameVariantExist = this.checkProductVariant(product);

        if (sameVariantExist) {
          this.alert = {
            isAlert: true,
            variant: "warning",
            msg: `${product.title} with the same variant already exist in Cart.`,
          };
          setTimeout(() => (this.alert = {}), 2000);
        } else {
          console.log(`variant not found`);
          this.saveToCart(product);
        }
      } else {
        this.saveToCart(product);
      }
    },
    isColorSizeTaken(product) { 
      if(product.selected_color && product.selected_size) return true;
      else return false;
    },
    checkProductVariant(product) {
      let raw = window.localStorage.getItem("cart");
      if (!raw) return false;
      let productsInCart = JSON.parse(raw);
      if (productsInCart && productsInCart.length > 0) {
        let result;
        productsInCart.map((p) => {
          if (result) return;
          if (product.id == p.id) {
            if (
              p.selected_color == product.selected_color &&
              p.selected_size == product.selected_size
            ) {
              result = true;
            } else {
              result = false;
            }
          }
        });
        return result;
      }
    },
    saveToCart(product) {
      product.quantity = 1; // setting init quantity before adding to cart
      product.product_total = product.current_price; // Setting Product_total to current Price right before adding it to cart
      this.cart.push(product);
      let stringed = JSON.stringify(this.cart);
      window.localStorage.setItem("cart", stringed);

      $nuxt.$emit("added-to-cart");

      this.alert = {
        isAlert: true,
        variant: "success",
        msg: `${product.title} has been added to the cart.`,
      };
      setTimeout(() => (this.alert = {}), 2000);
    },
    findProductVariant(product) {
      let variant = product.variants.filter((p) => {
        return (
          p.option1 == product.selected_color &&
          p.option2 == product.selected_size
        );
      });

      if (variant && variant.length > 0) {
        this.$set(product, "current_image", variant[0].featured_image.src);
        this.$set(product, "current_price", variant[0].price);
      }
    },

    attachColor(product_id, color) {
      let index = this.productsList.findIndex(
        (product) => product.id == product_id
      );
      this.$set(this.productsList[index], "is_color_selected", true);
      this.$nextTick(() => {
        this.$set(this.productsList[index], "selected_color", color);
        this.$set(this.productsList[index], "attr_step", 1);
      });

      if (
        this.productsList[index].selected_color &&
        this.productsList[index].selected_size
      ) {
        this.findProductVariant(this.productsList[index]);
      }
    },

    attachSize(product_id, size) {
      let index = this.productsList.findIndex(
        (product) => product.id == product_id
      );
      this.$set(this.productsList[index], "selected_size", size);
      if (
        this.productsList[index].selected_color &&
        this.productsList[index].selected_size
      ) {
        this.findProductVariant(this.productsList[index]);
      }
    },
    setInitImageAndPrice() {
      this.productsList.forEach((product) => {
        this.$set(
          product,
          "current_image",
          product.variants[0].featured_image.src
        );
        this.$set(product, "current_price", product.variants[0].price);
        this.$set(product, "product_total", product.variants[0].price);
      });
    },
  },
};

//TODO figure out a way that you can run the carousel only when you hover on it
</script>

<style lang="scss" scoped>
@import "../static/assets/hover.css";
@import "../static/assets/main.scss";

.product-image {
  width: 100%;
  height: 280px;
  object-fit: cover;
  object-position: 50% 50%;
}

.add-to-bag {
  background: transparent;
  border: 1px solid black;
  color: black;
  border-radius: 0px;
  font-size: 15px;
  text-align: center;
  display: block;
  margin-left: 5px;
  width: 96%;
  text-transform: uppercase;
  box-shadow: none;
}

.product-colors {
  display: flex;
}

.product-colors {
  width: 100%;
  span {
    width: 18px;
    border: 1px solid rgba(0, 0, 0, 0.5);
    height: 18px;
    margin: 0 7px;
    border-radius: 50%;
    cursor: pointer;
    padding: 5px;
  }

  .color-active {
    border: 5px solid black;
    transition: 0.3s;
  }
}

.product-sizes {
  width: 100%;

  span {
    border: 1px solid rgba(0, 0, 0, 0.5);
    margin: 5px;
    width: 20px;
    height: 20px;
    font-size: 1.7vh;
    cursor: pointer;
    padding: 3px 7px;
    display:inline;
  }

  .size-active {
    background: $primary-color;
    color: white;
    font-weight: 500;
  }
}

.product-box {
  .product-title {
    font-size: 14px;
    font-weight: 500;
  }

  .product-price {
    font-size: 16px;
    font-weight: 500;
    float: right;
  }
}

.nav-tabs .nav-link {
  border: none;
}

.product-box:hover .cart-btn-area {
  opacity: 1;
  transition: 0.5s;
}
</style>
