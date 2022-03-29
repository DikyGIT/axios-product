<template>
  <div>
    <section class="hero d-flex align-items-center">
      <div class="container">
        <div class="row d-flex align-items-center">
          <div
            class="
              col-xl col-lg col-md col-sm-12
              order-xl-0 order-lg-0 order-md-0 order-sm-1 order-1
            "
          >
            <h1><strong>Delicious The Foods</strong>, in your Gadget.</h1>
            <p>Ayo segera pilih dan pesan makanan favorit Anda!</p>
            <router-link to="/foods" class="btn btn-success btn-lg"
              ><b-icon-arrow-right class="mr-2"></b-icon-arrow-right
              >Pesan</router-link
            >
          </div>
          <div class="col mb-sm-5 mb-5">
            <img src="@/assets/img/hero.png" width="100%" alt="" />
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style scoped>
</style>