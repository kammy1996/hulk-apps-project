<template>
  <div>
    <SiteHeader />
    <Nuxt />
  </div>
</template>

<script>
import SiteHeader from "@/components/SiteHeader";

export default {
  name: "default",
  components: {
    SiteHeader,
  },
};
</script>

<style lang="scss" scoped>
* {
  font-family: "Poppins" !important;
}
</style>
