<template>
  <div class="home">
    <header-shayna />
    <banner-slider />
    <woman-slider />
    <social-media />
    <logo-partner />
    <footer-shayna />
  </div>
</template>

<script>
import HeaderShayna from "../components/HeaderShayna.vue";
import BannerSlider from "../components/BannerSlider.vue";
import WomanSlider from "../components/WomanSlider.vue";
import SocialMedia from "../components/SocialMedia.vue";
import LogoPartner from "../components/LogoPartner.vue";
import FooterShayna from "../components/FooterShayna.vue";
// @ is an alias to /src

export default {
  name: "Home",
  components: {
    HeaderShayna,
    BannerSlider,
    WomanSlider,
    SocialMedia,
    LogoPartner,
    FooterShayna,
  },
};
</script>
