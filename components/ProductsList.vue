<template>
  <div>
    <b-container>
      <b-row>
        <b-col lg="3" v-for="product in productsList" :key="product.id">
          <div class="product-box">
            <!-- figure out a way that you can run the carousel only when you hover on it -->
            <img width="100%" :src="product.current_image" alt="" />
            <div class="cart-btn-area">
              <div>
                <b-tabs
                  class="product-attrs"
                  v-model="product.attr_step"
                  small
                  fill
                  content-class="mt-3"
                  align="center"
                >
                  <b-tab class="product-attr-title" title="color" active
                    ><div class="product-colors">
                      <span
                        :style="{ background: color }"
                        :class="
                          product.selected_color == color ? 'color-active' : ''
                        "
                        v-for="color in product.options[0].values"
                        :key="color.id"
                        @click="attachColor(product.id, color)"
                      ></span></div
                  ></b-tab>
                  <b-tab
                    class="product-attr-title"
                    title="Size"
                    :disabled="!product.is_color_selected"
                  >
                    <div class="product-sizes" v-if="product.options[1]">
                      <span
                        class="cursor-pointer"
                        :class="
                          product.selected_size == size ? 'size-active' : ''
                        "
                        v-for="(size, index) in product.options[1] &&
                        product.options[1].values"
                        :key="index"
                        @click="attachSize(product.id, size)"
                        >{{ size }}
                      </span>
                    </div>
                    <div v-else>No size available</div>
                  </b-tab>
                </b-tabs>
                <div class="height-20"></div>
                <b-button
                  class="mb-2"
                  v-if="product.selected_color && product.selected_size"
                  @click="addToCart(product)"
                  ><b-icon class="mr-2" icon="bag" scale="0.8"></b-icon>Add to
                  Bag</b-button
                >
              </div>
            </div>

            <div class="height-10"></div>

            <span class="product-title">{{ product.title }} </span>
            <span class="wishlist-icon cursor-pointer"
              ><b-icon icon="heart" scale="1.2"></b-icon
            ></span>
            <span class="product-desc"
              >Captivate with this shirt’s versatile urban look that works as
              well at happy hour as it does in the back yard
            </span>
            <p class="product-price">$ {{ product.current_price }}</p>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import ProductsList from "@/models/productsList";
import _ from "lodash";

export default {
  name: "ProductsList",
  data() {
    return {
      productsList: ProductsList,
      cart: [],
    };
  },
  mounted() {
    this.setInitImageAndPrice();
  },
  computed: {},
  methods: {
    showImageAsPerColor() {},
    addToCart(product) {
      let raw = window.localStorage.getItem("cart");
      let parsed;
      if (raw) {
        parsed = JSON.parse(raw);
        this.cart = parsed;
        let productFound = this.cart.findIndex((p) => p.id === product.id);

        if (productFound >= 0) {
          alert(`${product.title} already exist in the cart`);
        } else {
          this.saveToCart(product);
        }
      } else {
        this.saveToCart(product);
      }
    },
    saveToCart(product) {
      product.quantity = 1; // setting init quantity before adding to cart
      product.product_total = product.current_price; // Setting Product_total to current Price right before adding it to cart
      this.cart.push(product);
      let stringed = JSON.stringify(this.cart);
      window.localStorage.setItem("cart", stringed);
      
      $nuxt.$emit("added-to-cart");
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

.cart-btn-area {
  opacity: 0;
  background: white;
  position: absolute;
  width: 100%;
  left: 0px;
  bottom: 110px;
  text-align: center;
  z-index: 1;
  display: block;
  margin: auto;
  padding: 10px;

  button {
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
    justify-content: center;
  }

  .product-colors {
    width: 100%;
    span {
      width: 20px;
      border: 1px solid rgba(0, 0, 0, 1);
      height: 20px;
      margin: 0 7px;
      cursor: pointer;
      padding: 5px;
    }

    .color-active {
      border: 3px solid black;
    }
  }

  .product-sizes {
    width: 100%;

    span {
      border: 1px solid rgba(0, 0, 0, 0.5);
      margin: 5px;
      width: 20px;
      font-size: 13px;
      cursor: pointer;
      padding: 5px 7px;
    }
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
    font-weight: 600;
    text-transform: uppercase;
  }

  .product-price {
    font-size: 18px;
    font-weight: 700;
  }

  .wishlist-icon {
    float: right;
  }

  .product-desc {
    width: 200px;
    overflow: hidden;
    display: inline-block;
    text-overflow: ellipsis;
    white-space: nowrap;
    font-size: 13px;
    color: grey;
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
