<template>
  <div class="main">
    <div class="bg" v-bind:style="{ backgroundImage: 'url(' + wrapImgUrl(background.files[background.current]) + ')' }">
      <h1>Калькулятор рассчета вида и стоимости бетонного забора</h1>
      <app-background></app-background>
      <app-controls></app-controls>
      <app-fence v-bind:fenceData="fenceData" :key="fenceKey"></app-fence>
      <app-price-calculator></app-price-calculator>
    </div>
  </div>
</template>

<script>
import Background from './components/Background.vue';
import Controls from './components/Controls.vue';
import Fence from './components/Fence.vue';
import Price from './components/Price.vue';
import { EventBus } from './main';
import json from './FenceData.json';

export default {
  name: 'app',
  components: {
    'app-background': Background,
    'app-controls': Controls,
    'app-fence': Fence,
    'app-price-calculator': Price
  },
  data() {
    return {
      background: {
        files: ["_bg1.jpg", "_bg2.jpg", "_bg3.jpg"],
        current: 0
      },
      fenceData: json,
      fenceKey: 0
    }
  },
  methods: {
    getImgUrl(pic) {
      return require('./assets/'+pic)
    },
    wrapImgUrl: function(img_url) {
      return "'" + this.getImgUrl(img_url) + "'";
    }
  },
  created() {
    EventBus.$on('fenceUpdated', () => {
    })
  } 
}
</script>

<style lang="scss">
body {
  background: #666;
}
.main-container {
  background: #fff;
  width: 935px;
  margin: 100px auto 100px;
  border-radius: 12px;
  overflow:hidden;
}
h1 {
  margin: 0;
  color: #a10000;
  font-size: 1.2em;
  font-weight: bold;
  text-transform: uppercase;
  text-align: center;
  padding: 36px 0 0 0;
}
.main {
  position: relative;
  font-family: "PT Sans", sans-serif;
  width: 100%;
  min-height: 710px;
  .bg {
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
  }
}
</style>
