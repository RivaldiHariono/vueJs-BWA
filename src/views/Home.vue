<template>
  <div class="home">
    <headerAldi />
    <heroAldi />
    <bannerAldi />
    <instaAldi />
    <partnerAldi />
    <footerAldi />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import headerAldi from '@/components/headerAldi.vue';
import heroAldi from '@/components/heroAldi.vue';
import bannerAldi from '@/components/bannerAldi.vue';
import instaAldi from '@/components/instaAldi.vue';
import partnerAldi from '@/components/partnerAldi.vue';
import footerAldi from '@/components/footerAldi.vue';
export default {
  name: 'Home',
  components: {
    headerAldi,
    heroAldi,
    bannerAldi,
    instaAldi,
    partnerAldi,
    footerAldi
  }
}
</script>
