<template>
  <!-- Header Section Begin -->
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service">
            <i class="fa fa-envelope"></i> januarhairul@gmail.com
          </div>
          <div class="phone-service">
            <i class="fa fa-phone"></i> +62 8963 979 1889
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <router-link to="/">
                <img src="img/logo_website_shayna.png" alt="" />
              </router-link>
            </div>
          </div>
          <div class="col-lg-7 col-md-7"></div>
          <div class="col-lg-3 text-right col-md-3">
            <ul class="nav-right">
              <li class="cart-icon">
                Keranjang Belanja &nbsp;
                <router-link to="/cart">
                  <i class="icon_bag_alt"></i>
                  <span>{{ keranjangUser.length }}</span>
                </router-link>
                <div
                  :key="componentKey"
                  class="cart-hover"
                  v-if="keranjangUser.length > 0"
                >
                  <div class="select-items">
                    <table>
                      <tbody>
                        <tr
                          v-for="(keranjang, index) in keranjangUser"
                          :key="keranjang.id"
                        >
                          <td class="si-pic">
                            <img
                              class="photo-cart-item"
                              :src="keranjang.photo"
                              alt=""
                            />
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>{{ rupiah(keranjang.price) }}</p>
                              <h6>{{ keranjang.name }}</h6>
                            </div>
                          </td>
                          <td class="si-close" @click="removeCartItem(index)">
                            <i class="ti-close"></i>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                  <div class="select-total">
                    <span>total:</span>
                    <h5>{{ rupiah(totalHarga) }}</h5>
                  </div>
                  <div class="select-button">
                    <router-link to="/cart" class="primary-btn view-card"
                      >VIEW CARD</router-link
                    >
                    <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                  </div>
                </div>
                <div class="cart-hover" v-else>
                  <div class="select-items">
                    <table>
                      <tbody>
                        <tr class="text-center">
                          <td>Keranjang kosong</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
  <!-- Header End -->
</template>

<script>
export default {
  name: "HeaderShayna",
  data() {
    return {
      keranjangUser: [],
      componentKey: "",
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
    forceRerender() {
      this.componentKey += 1;
    },
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
    },
  },
  computed: {
    totalHarga() {
      return this.keranjangUser.reduce(function (items, data) {
        return items + data.price;
      }, 0);
    },
  },
};
</script>

<style scoped>
.photo-cart-item {
  max-width: 100px;
}
</style>