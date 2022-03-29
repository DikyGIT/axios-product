<template>
  <div class="food-detail">
    <!-- <h1>Food Detail {{ $route.params.id }}</h1> -->

    <Navbar />

    <b-container>
      <!-- Breadcrumb -->
      <b-row class="mt-5">
        <b-col>
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Foods Order
              </li>
            </ol>
          </nav>
        </b-col>
      </b-row>

      <b-row class="mt-3">
        <div class="col-md-6">
          <img
            :src="require(`@/assets/img/${product.gambar}`)"
            alt=""
            class="img-fluid shadow detailImg"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga : <strong>Rp.{{ product.harga }}</strong>
          </h4>
          <form @submit.prevent class="mt-3">
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan</label>
              <input
                type="number"
                class="form-control"
                placeholder="Masukkan Jumlah Pesanan"
                v-model="pesan.jumlah_pemesanan"
                required
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                class="form-control"
                name="keterangan"
                id="keterangan"
                placeholder="Keterangan spt : Pedas, Nasi Setengah ..."
                v-model="pesan.keterangan"
                required
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";

import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },

  data() {
    return {
      product: {},
      pesan: {},
    };
  },

  // created() {
  //   this.getDetailProducts();
  // },

  created() {
    this.getDetailProducts();
  },

  methods: {
    getDetailProducts: async function () {
      try {
        const response = await axios.get(
          "http://localhost:3000/products/" + this.$route.params.id
        );
        console.log(response);
        this.product = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan) {
        this.pesan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Sukses Masuk Keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Jumlah Pesanan Kosong ", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },

  // mounted() {
  //   axios
  //     .get("http://localhost:3000/products/" + this.$route.params.id)
  //     .then((response) => console.log(response))
  //     .catch((error) => console.error(error));
  // },
};
</script>

<style scoped>
</style>