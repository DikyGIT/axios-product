<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Foods</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <b-input-group class="mt-3">
            <b-form-input v-model="paramsProduct" placeholder="Cari Makanan Kesukaan Anda..." @keyup="searchProducts"></b-form-input>
            <b-input-group-append>
              <template>
                <b-input-group-text><b-icon-search variant="success"></b-icon-search></b-input-group-text>
              </template>
            </b-input-group-append>
          </b-input-group>
        </div>
      </div>

      <div class="row mb-4" v-if="products.length">
        <div class="col-md-4 mt-4" v-for="product in products" :key="product.id">
          <CardProduct :product="product" />
        </div>
      </div>
      <div v-else>
        <p class="text-center mt-5 fw-bold" style="color: red">Data Makanan Tidak Ada!</p>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import CardProduct from "@/components/CardProduct";

// * sebelum menggunakan axios, registrasikan terlebih dahulu axios tersebut dengan mengimportnya
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      // * variable product untuk menyimpan data yang dipanggil dari API
      products: [],
      // * variable paramsProduct untuk meyimpan parameter yang di ketik pada input search
      paramsProduct: "",
    };
  },

  // * created berfungsi untuk membuat sebuat method yang nantinya akan dipanggil di fungsi methods
  created() {
    this.getProducts();
    this.searchProducts();
  },

  // * methods berfungsi untuk membuat fungsi apa yang akan dilakukan pada method yang sudah dibuat di dalam created
  methods: {
    // * function yang menggunakan asynchronous, tandanya sebelum function ada async, async biasanya bersandingan dengan await, dan untuk error handling nya menggunakan try and catch, didalam try codingan akan dijalankan dengan bantuan await, kemudian di dalam catch akan mengambil error, jika didalam try ada error, cara bacanya buat fungsi getProducts dengan menggunakan async function(), kemudian didalam function tersebut akan ada errorhandling try and catch, didalam try, codingan akan dijalankan terlebih dahulu, jika berjasil maka sintaks akan berjalan seperti biasa, jika error maka akan di alihkan ke catch, kemudian error tersebut akan ditampilkan di console log
    getProducts: async function () {
      try {
        // * buat variable response atau yang lain, yang berfungsi untuk menyimpan sebuah fungsi axios.get(endPoint dari sebuah API), sebelum axios terlebih dahulu gunakan await, karna fungsi getProducts menggunakan asynchronous
        const response = await axios.get("http://localhost:3000/products");
        // * simpan variabel response kedalam console.log(response), agar data yang dipanggil, terbaca di console
        console.log(response);
        // * panggil data products yang sudah di buat di fungsi data(), kemudian simpan data yang ada di API kedalam variable products.
        this.products = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    searchProducts: async function () {
      try {
        const response = await axios.get(`http://localhost:3000/products?q=${this.paramsProduct}`);
        console.log(response);
        this.products = response.data;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style scoped></style>
