<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
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
                Keranjang
              </li>
            </ol>
          </nav>
        </b-col>
      </b-row>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(keranjang, index) in keranjangs" :key="index">
                  <th scope="row">
                    {{ index + 1 }}
                  </th>
                  <td>
                    <img
                      :src="`../../assets/img/${keranjang.products.gambar}`"
                      class="img-fluid"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td>Rp. {{ keranjang.products.harga }}</td>
                  <td>
                    <strong
                      >Rp.
                      {{
                        keranjang.products.harga * keranjang.jumlah_pemesanan
                      }}</strong
                    >
                  </td>
                  <td class="text-center text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang.id)"
                      class="trash"
                    ></b-icon-trash>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" class="text-right">
                    <strong>Total Harga :</strong>
                  </td>
                  <td>
                    <strong>Rp. {{ totalHarga }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- ? Form Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form @submit.prevent class="mt-3">
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input
                type="text"
                class="form-control"
                placeholder="Masukkan Nama Pemesan"
                v-model="pesan.nama"
                required
              />
            </div>
            <div class="form-group">
              <label for="noMeja">No Meja :</label>
              <input
                type="text"
                class="form-control"
                placeholder="Masukkan Nomor Meja Pemesan"
                v-model="pesan.noMeja"
                required
              />
            </div>

            <button
              type="submit"
              class="btn btn-success float-right"
              @click="checkout"
            >
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </b-container>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },

  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },

  created() {
    this.setKeranjangs();
  },

  methods: {
    setKeranjangs: async function () {
      try {
        const response = await axios.get("http://localhost:3000/keranjangs/");
        console.log(response);
        this.keranjangs = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then((response) => {
          console.log(response);
          // this.$router.push({ path: "/keranjang" });
          this.$toast.error("Item berhasil di hapus ", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          this.setKeranjangs();
        })
        .catch((error) => console.error(error));
    },
    checkout() {
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjang;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            // ? Hapus Semua Keranjang
            this.keranjangs.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.error(error));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Sukses dipesan", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Nama dan noMeja harus di isi ", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },

  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        // ? parameter pertama yaitu items berfungsi untuk mengambil indexnya
        // ? parameter kedua yaitu data berfungsi untuk mengambil isi datanya yang ada didalam index
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },

  // methods: {
  //   setKeranjangs(data) {
  //     this.keranjangs = data;
  //   },
  // },

  // mounted() {
  //   axios
  //     .get("http://localhost:3000/keranjangs/")
  //     .then((response) => this.setKeranjangs(response.data))
  //     .catch((error) => console.error(error));
  // },
};
</script>

<style scoped></style>
