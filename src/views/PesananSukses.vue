<template>
  <div class="pesanan-sukses">
    <Navbar />
    <b-container>
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/img/menunggu.png" width="300" alt="" />
          <h2><strong>Sukses!</strong></h2>
          <h4>
            Pesanan Anda segera diproses <br />
            Mohon menunggu
          </h4>
        </div>
      </div>
    </b-container>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";

export default {
  name: "PesananSukses",
  components: {
    Navbar,
  },
};
</script>

<style scoped>
</style>