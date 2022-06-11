<template>
  <div>
    <div class="height-50"></div>
      <div class="heading text-center">
        <h3 >Featured Collection</h3>
        <p>
          Some of our best collections.
        </p>
      </div>
      <div class="height-50"></div>
      <template class="products">
        <ProductsList />
      </template>

      <template class="shopping-cart" >
        <Cart />
      </template>
      <div class="height-100"></div>
  </div>
</template>

<script>
import ProductsList from '@/components/ProductsList';
import Cart from '@/components/Cart'

export default {
  name:'Index',
  components:  {
    ProductsList,
    Cart
  }
};


//TODO: Need to implement color Variant as per pricing and Image 

</script>

<style lang="scss" scoped>

</style>


