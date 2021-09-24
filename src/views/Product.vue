<template>
  <div class="product">
    <header-shayna />

    <bread-crumb title="Detail Produk" />

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
                <div
                  class="product-thumbs"
                  v-if="productDetail.galleries.length > 0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :nav="false"
                    :dots="false"
                    :margin="10"
                  >
                    <div
                      v-for="gallery in productDetail.galleries"
                      :key="gallery.id"
                      class="pt"
                      @click="changeImage(gallery.photo)"
                      :class="gallery.photo == gambar_default ? 'active' : ''"
                    >
                      <img :src="gallery.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetail.type }}</span>
                    <h3>{{ productDetail.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    {{ productDetail.description }}
                    <h4 class="mt-3">{{ rupiah(productDetail.price) }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart" class="primary-btn pd-cart"
                      >Add To Cart</router-link
                    >
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <related-product />

    <footer-shayna />
  </div>
</template>

<script>
import HeaderShayna from "../components/HeaderShayna.vue";
import FooterShayna from "../components/FooterShayna.vue";
import carousel from "vue-owl-carousel";
import RelatedProduct from "../components/RelatedProduct.vue";
import BreadCrumb from "../components/BreadCrumb.vue";
import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    FooterShayna,
    carousel,
    RelatedProduct,
    BreadCrumb,
  },
  data() {
    return {
      gambar_default: "",
      productDetail: [],
    };
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.getGalleryProduct(res.data.data))
      .catch((err) => console.log(err));
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },
    rupiah(value) {
      let val = (value / 1).toFixed(2).replace(".", ",");
      return "Rp. " + val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
    getGalleryProduct(data) {
      this.productDetail = data;
      this.gambar_default = data.galleries[0].photo;
    },
  },
};
</script>
