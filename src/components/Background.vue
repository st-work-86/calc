<template>
    <div class="bg-switcher">
        <div class="bg-switcher__left" v-on:click.prevent="backgroundPrev()">Previous Background</div>
        <div class="bg-switcher__center"></div>
        <div class="bg-switcher__right" v-on:click.prevent="backgroundNext()">Next Background</div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  },
  methods: {
    backgroundNext: function() {
      if (this.$parent.background.files.length-1 == this.$parent.background.current) {
        this.$parent.background.current = 0;
      } else {
        this.$parent.background.current++;
      }
    },
    backgroundPrev: function() {
      if (this.$parent.background.current == 0) {
        this.$parent.background.current = this.$parent.background.files.length-1
      } else {
        this.$parent.background.current--;
      }
    }
  }
}
</script>

<style scoped lang="scss">
.bg-switcher {
  position: relative;
  margin: 40px auto 0 auto;
  width: 66px;
  height: 66px;  
}
.bg-switcher__center {
  width: 66px;
  height: 66px;
  background: rgba(0,0,0,0.5) url('./../assets/interface/home.png') 50% 50% no-repeat;
  border-radius: 50%;
}
.bg-switcher__left, .bg-switcher__right {
  position: absolute;
  width: 17px;
  height: 32px;
  background: url('./../assets/interface/home-arrows.png') no-repeat;
  overflow: hidden;
  text-indent: -99999px;
  top: 17px;
  cursor: pointer;
  &:hover {
    background-position: 0 -32px;
  }
}
.bg-switcher__left {
  left: -30px;
}
.bg-switcher__right {
  transform: rotate(180deg);
  right: -30px;
}
</style>