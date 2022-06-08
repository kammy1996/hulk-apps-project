<template>
  <div>
    <b-container>
      <b-row>
        <b-col lg="3">
          <div class="product-box">
            <!-- figure out a way that you can run the carousel only when you hover on it -->
            <b-carousel ref="product-carousel" :interval="3000">
              <b-carousel-slide
                img-src="https://cdn.shopify.com/s/files/1/0581/7663/5061/products/3_a13a42c9-8061-4ec2-a3be-c70e97223be6.jpg?v=1638523968"
              ></b-carousel-slide>
              <b-carousel-slide
                img-src="https://cdn.shopify.com/s/files/1/0581/7663/5061/products/2_564ec0cb-4187-4e56-a1be-872508526f4f.jpg?v=1638523968"
              ></b-carousel-slide>
              <b-carousel-slide
                img-src="https://cdn.shopify.com/s/files/1/0581/7663/5061/products/3_a13a42c9-8061-4ec2-a3be-c70e97223be6.jpg?v=1638523968"
              ></b-carousel-slide>
              <b-carousel-slide
                img-src="https://cdn.shopify.com/s/files/1/0581/7663/5061/products/3_a13a42c9-8061-4ec2-a3be-c70e97223be6.jpg?v=1638523968"
              ></b-carousel-slide>
            </b-carousel>

            <div class="cart-btn-area">
              <b-button class="mb-2"
                ><b-icon class="mr-2" icon="bag" scale="0.8"></b-icon>Add to
                Bag</b-button
              >

               <div class="shoes-colors">
                  <span class="blue active"></span>
                  <span class="green"  ></span>
                  <span class="yellow" ></span>
                  <span class="rose" ></span>
                  <span class="rose"  ></span>
                </div>
            </div>

            <div class="height-10"></div>

            <span class="product-title">Basic jumper for women</span>
            <span class="wishlist-icon cursor-pointer"
              ><b-icon icon="heart" scale="1.2"></b-icon
            ></span>
            <span class="product-desc"
              >Captivate with this shirt’s versatile urban look that works as
              well</span
            >
            <p class="product-price">$10</p>
          </div>
        </b-col>
        
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "ProductsList",
  data() {
    return {};
  },
  mounted() {
    // this.playCarouselImages()
  },
  methods: {
    // playCarouselImages() {
    //   this.$refs[`product-carousel`].interval = 2000
    // }
  },
};

//TODO figure out a way that you can run the carousel only when you hover on it
</script>

<style lang="scss" scoped>
@import "../static/assets/hover.css";

.cart-btn-area {
  opacity: 0;
  height: 80px;
  background: white;
  position: absolute;
  width: 100%;
  left: 0px;
  bottom: 100px;
  text-align: center;
  z-index: 99;
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

  .shoes-colors{
  margin-bottom: 40px;
  display: flex;
  justify-content: center;
}

  .shoes-colors span{
    width: 14px;
    background:green;
    height: 14px;
    margin: 0 10px;
    border-radius: 50%;
    cursor: pointer;
    position: relative;
  }

  .shoes-colors .active { 
    border:2px solid black;
    padding:5px;
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
