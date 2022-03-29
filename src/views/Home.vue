<template>
  <div class="home">
    <Navbar />
    <Hero />
    <b-container>
      <div class="row mt-2">
        <div class="col">
          <h2>Best <strong>Foods</strong></h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right"><b-icon-eye></b-icon-eye>Lihat Semua</router-link>
        </div>
      </div>

      <div class="row mb-3">
        <div class="col-md-4 mt-4" v-for="product in products" v-bind:key="product.id">
          <CardProduct :product="product" />
        </div>
      </div>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import CardProduct from "@/components/CardProduct";

// * Untuk memakai axios, terlebih dahulu harus di registrasikan
import axios from "axios";

export default {
  name: "Home",
  components: {
    Navbar,
    Hero,
    CardProduct,
  },
  data() {
    return {
      // * Variable yang berfungsi untuk menyimpan data API
      products: [],
    };
  },

  created() {
    // * Membuat sebuah method getProduct() yang akan di panggil di methods
    this.getProducts();
  },

  methods: {
    // * Fungsi dari method yang sudah dibuat di created()
    // * Jika function menggunakan asynchronus maka harus ada try kemudian didalam try harus ada await, kemudian catch untuk menangkap error, jika didalam try ada yang error, berikut dibawah ini merupakan fungsi yang diberikan asynchronus, tandanya sebelum function diberikan async, kemudian untuk sintaks yang ada didalam try, sintaks tersebut yang akan dijalan terlebih dahulu, jika sintaks yang ada didalam try ada yang error, maka akan lompat ke catch, kemudian menampilkan sintaks error tersebut di console log
    getProducts: async function () {
      try {
        const response = await axios.get("http://localhost:3000/best-products");
        console.log(response);
        this.products = response.data;
      } catch (error) {
        console.error(error);
      }
    },
  },

  // methods: {
  //   setProducts(data) {
  //     this.products = data;
  //   },
  // },

  // mounted() {
  //   async function getUser() {
  //     try {
  //       const response = await axios.get("http://localhost:3000/best-products");
  //       console.log(response);
  //     } catch (error) {
  //       console.error(error);
  //     }
  //   }
  //   getUser();
  // },
};
</script>
