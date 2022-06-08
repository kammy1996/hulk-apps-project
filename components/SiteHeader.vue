<template>
  <div>
    <b-navbar variant="light" type="light">
      <b-container>
        <b-navbar-brand class="mt-2 mb-2" href="#">
          <img width="80%" src="../static/assets/images/HA_Logo.svg" />
        </b-navbar-brand>

        <b-navbar-nav class="ml-auto">
          <span>
            <b-icon icon="person" class="mr-4" scale="1.7"></b-icon>
          </span>

          <span class="cursor-pointer" @click="openCart">
            <b-icon icon="bag" scale="1.4"></b-icon>
            <span class="cart-count badge-primary badge-pill">4</span>
          </span>
        </b-navbar-nav>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "SiteHeader",
  data() {
    return {};
  },
  methods: {
    openCart() {
      $nuxt.$emit('toggle-cart')
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../static/assets/main.scss";

.cart-count {
  padding: 1px 6px;
  font-size: 12px;
  position: relative;
  bottom: 10px;
  left: -7px;
  background: $primary-color;
}
</style>
