<template>
  <div class="product">
    <header-shayna />

    <bread-crumb title="Keranjang" />
    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="(keranjang, index) in keranjangUser"
                        :key="keranjang.id"
                      >
                        <td class="cart-pic first-row">
                          <img :src="keranjang.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ keranjang.name }}</h5>
                        </td>
                        <td class="p-price first-row">
                          {{ rupiah(keranjang.price) }}
                        </td>
                        <td class="delete-item">
                          <a @click="removeCartItem(index)" href="#"
                            ><i class="material-icons"> close </i></a
                          >
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4 text-left">Informasi Pembeli:</h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        v-model="customerInfo.name"
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        v-model="customerInfo.email"
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        v-model="customerInfo.phone_number"
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea
                        v-model="customerInfo.address"
                        class="form-control"
                        id="alamatLengkap"
                        rows="3"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">
                      ID Transaction <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal <span>{{ rupiah(totalHarga) }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak (10%)
                      <span>{{ rupiah(pajak) }} </span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya
                      <span>{{ rupiah(totalBiaya) }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer <span>BRI</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening <span>4809 0100 8457530</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima <span>Hairul Januar</span>
                    </li>
                  </ul>
                  <a @click="checkout()" href="#" class="proceed-btn"
                    >I ALREADY PAID</a
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->

    <footer-shayna />
  </div>
</template>

<script>
import HeaderShayna from "../components/HeaderShayna.vue";
import FooterShayna from "../components/FooterShayna.vue";
import BreadCrumb from "../components/BreadCrumb.vue";
import axios from "axios";
export default {
  name: "Product",
  components: {
    HeaderShayna,
    FooterShayna,
    BreadCrumb,
  },
  data() {
    return {
      gambar_default: "",
      keranjangUser: [],
      customerInfo: {
        name: "",
        email: "",
        phone_number: "",
        address: "",
      },
    };
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
  },
  methods: {
    rupiah(value) {
      let val = (value / 1).toFixed(0).replace(".", "");
      return "Rp. " + val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
    removeCartItem(index) {
      this.keranjangUser.splice(index, 1);
      this.refreshCart();
    },
    refreshCart() {
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
      window.location.reload();
    },
    checkout() {
      let productIds = this.keranjangUser.map(function (product) {
        return product.id;
      });

      let checkoutData = {
        name: this.customerInfo.name,
        email: this.customerInfo.email,
        phone_number: this.customerInfo.phone_number,
        address: this.customerInfo.address,
        transaction_total: this.totalBiaya,
        transaction_status: "PENDING",
        transaction_details: productIds,
      };

      axios
        .post("http://127.0.0.1:8000/api/checkout", checkoutData)
        .then(() => this.$router.push("success"))
        .catch((err) => console.log(err));
    },
  },
  computed: {
    totalHarga() {
      return this.keranjangUser.reduce(function (items, data) {
        return items + data.price;
      }, 0);
    },
    pajak() {
      return (this.totalHarga * 10) / 100;
    },
    totalBiaya() {
      return this.totalHarga + this.pajak;
    },
  },
};
</script>
