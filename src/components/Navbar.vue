<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <b-container>
        <b-navbar-brand to="/">Kulineran</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ml-auto">
            <router-link class="nav-link" to="/">Home</router-link>
            <router-link class="nav-link" to="/foods">Foods</router-link>
          </b-navbar-nav>

          <b-navbar-nav class="ml-auto">
            <router-link class="nav-link" to="/keranjang">
              Keranjang
              <b-icon-bag></b-icon-bag>
              <span class="badge badge-success ml-2">{{
                updateKeranjang
                  ? updateKeranjang.length
                  : jumlah_pesanans.length
              }}</span>
            </router-link>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  props: ["updateKeranjang"],
  data() {
    return {
      jumlah_pesanans: [],
    };
  },
  created() {
    this.setJumlah();
  },
  methods: {
    setJumlah: async function () {
      try {
        const response = await axios.get("http://localhost:3000/keranjangs");
        console.log(response);
        this.jumlah_pesanans = response.data;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped></style>
