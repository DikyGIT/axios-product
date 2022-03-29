<template>
  <div class="d-flex justify-content-center">
    <div class="card shadow card-product" style="width: 18rem">
      <img
        :src="require(`@/assets/img/${product.gambar}`)"
        class="card-img-top"
        alt=""
      />
      <div class="card-body">
        <h5 class="card-title">{{ product.nama }}</h5>
        <p class="card-text">Harga: Rp. {{ product.harga }}</p>
        <router-link :to="`/foods/${product.id}`" class="btn btn-success"
          ><b-icon-cart class="mr-2"></b-icon-cart>Pesan</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>

<style scoped>
</style>