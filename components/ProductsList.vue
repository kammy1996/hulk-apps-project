<template>
  <div>
    <b-container>
      <b-row>
        <b-col lg="3" v-for="product in productsList" :key="product.id">
          <div class="product-box">
            <!-- figure out a way that you can run the carousel only when you hover on it -->
            <img width="100%" :src="product.images[0].src" alt="" />
            <div class="cart-btn-area">
              <b-button class="mb-2" @click="addToCart(product)"
                ><b-icon class="mr-2" icon="bag" scale="0.8"  ></b-icon>Add to
                Bag</b-button
              >

              <b-row>
                <b-col cols="3">
                  <div class="product-colors">
                   
                    <span
                      :style="{ background: color }"
                      v-for="color in product.options[0].values"
                      :key="color.id"
                      @click="showImageAsPerColor(product.id, color)"
                    ></span>
                  </div>
                </b-col>
                <b-col cols="9">
                  <div class="product-sizes mt-n2">
                      <b-avatar variant="light"  size="30px" v-for="(size,index) in product.options[1] && product.options[1].values"
                      :key="index"> {{ size}}</b-avatar>
                  </div>
                </b-col>
              </b-row>
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
            <p class="product-price">$10</p>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import ProductsList from "@/models/productsList";

export default {
  name: "ProductsList",
  data() {
    return {
      productsList: ProductsList,
      cart: []
    };
  },
  mounted() {
    // this.playCarouselImages()
  },
  computed: {},
  methods: {
    // playCarouselImages() {
    //   this.$refs[`product-carousel`].interval = 2000
    // }
    showImageAsPerColor() {},
    addToCart(product)  {
      let raw = window.localStorage.getItem("cart")
      let parsed;
      if(raw) { 
        parsed = JSON.parse(raw)
        this.cart = parsed;
        let productFound = this.cart.findIndex(p => p.id === product.id)

        if(productFound >= 0) { 
          alert(`${product.title} already exist in the cart`)
        } else { 
        this.saveToCart(product)
        }
      } else { 
        this.saveToCart(product)
      }


    },
    saveToCart(product) { 
      this.cart.push(product)
      let stringed = JSON.stringify(this.cart)
      window.localStorage.setItem("cart",stringed)

      $nuxt.$emit('added-to-cart')
    }
  },
};

//TODO figure out a way that you can run the carousel only when you hover on it
</script>

<style lang="scss" scoped>
@import "../static/assets/hover.css";

.cart-btn-area {
  opacity: 0;
  height: 90px;
  background: white;
  position: absolute;
  width: 100%;
  left: 0px;
  bottom: 120px;
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
    margin-bottom: 40px;
    display: flex;
    justify-content: center;
  }

  .product-colors {
      width:100%;
      span {
      width: 14px;
      border: 1px solid rgba(0, 0, 0, 0.5);
      height: 14px;
      margin: 0 5px;
      border-radius: 50%;
      cursor: pointer;
    }
  }

  .product-colors.active {
    border: 2px solid black;
    padding: 5px;
  }

  .product-sizes { 
    width:100%;

    span {
      border: 1px solid rgba(0, 0, 0, 0.5);
      margin: 5px;
      font-size:13px;
      border-radius: 50%;
      cursor: pointer;
      padding:5px 7px;
    }
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

.product-box:hover .cart-btn-area {
  opacity: 1;
  transition: 0.5s;
}
</style>
