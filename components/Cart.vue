<template>
  <div>
    <b-sidebar
      v-model="isSidebar"
      id="sidebar-right"
      right
      width="450px"
      shadow
    >
      <div class="empty-bag">

      </div>
      <div class="cart-area">
        <p class="cart-title pl-4">
          <span class="cart-heading">Your Bag </span>
          <span class="cart-items">(2 Items)</span>
        </p>

        <div class="product-preview pl-4">
          <b-row>
            <b-col cols="3">
              <b-img class="preview-img" width="100%" src="https://cdn.shopify.com/s/files/1/0581/7663/5061/products/3_a13a42c9-8061-4ec2-a3be-c70e97223be6.jpg?v=1638523968" ></b-img>
            </b-col>
            <b-col cols="5">
              <div class="product-preview-info ml-2" >
                <span class="preview-title" >Basic jumper for women</span><br>
                <span class="preview-color" >Color: Green</span><br>
                <div class="height-10"></div>
                <b-input-group size="sm" class="quantity-input"> 
                  <b-input-group-prepend >
                    <b-btn  class="quantity-btn mr-n2" @click="decrement()">-</b-btn>
                  </b-input-group-prepend>

                  <b-form-input class="input-box" type="number" min="0.00" :value="quantity"></b-form-input>

                  <b-input-group-append>
                    <b-btn  class="quantity-btn ml-n3"  @click="increment()">+</b-btn>
                  </b-input-group-append>
                </b-input-group>
              </div>
            </b-col>
            <b-col cols="4">
             <div class="text-right pr-3" >
                <p><b-icon icon="trash" class="cursor-pointer remove-icon" scale="1">X</b-icon></p>
                <div class="height-50"></div>
                <p class="preview-price">$99</p>
              </div>
            </b-col>
          </b-row>
        </div>
        <div class="height-20"></div>
        <div class="cart-footer pl-5">
          <b-row>
            <p class="cart-footer-title">SubTotal</p>
            <p class="cart-footer-total ">$200</p>
          </b-row>
        </div>
      </div>


    </b-sidebar>
  </div>
</template>

<script>
export default {
  name: "Cart",
  data() {
    return {
      isSidebar: true,
      quantity: 1
    };
  },
  created() {
    this.$nuxt.$on("toggle-cart", () => {
      this.isSidebar = true;
    });
  },
  methods :  {
    increment() {
      this.quantity++;
    },
    decrement() {
      if (this.quantity === 1) {
        alert("Negative quantity not allowed");
      } else {
        this.quantity--;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.cart-area {

  .product-preview { 
    border-bottom:1px solid rgba(0,0,0,0.2);
    padding-bottom:15px;
    margin:0px 10px;
  }

  .cart-heading {
    font-size: 20px;
    font-weight: 600;
    text-transform: uppercase;
  }

  .preview-title { 
    font-size:14px;
    text-transform:uppercase;
    font-weight: 600;
  }

  .preview-color { 
    font-size:13px;
  }

  .preview-price { 
    font-weight:600;
  }

  .quantity-btn { 
    padding:0px 10px;
    background:rgba(0,0,0,0.1);
    border:none;
    border-radius:100px;
    color:black;
    font-weight:500;
    box-shadow:none !important;
  }

  .quantity-input { 
    width:110px;
    input[type=number]::-webkit-inner-spin-button, 
    input[type=number]::-webkit-outer-spin-button { 
      -webkit-appearance: none; 
    }

    .input-box { 
      background:transparent;
      border:none;
      padding:0px 30px
    }

    .input-box:focus { 
      box-shadow: none;
    }
  }


  
}
</style>
