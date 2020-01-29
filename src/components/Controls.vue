<template>
  <div class="controls-switcher">
    <div class="row">
      <div class="col col-xs-12">Укажите кол-во секций(0,5м)
        <select v-model="options.sections" v-on:change="updateOptions()">
          <option v-for="i in 5" v-bind:key="i">{{i}}</option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="col col-xs-12">
        <label class="controls-checkbox">
          <input type="checkbox" v-model="options.peak" v-on:change="updateOptions()" />
          <span class="text">Нужен ли конек</span>
          <span class="check"></span>
        </label>
      </div>
    </div>
    <div class="row">
      <div class="col col-xs-12">
        <label class="controls-checkbox">
          <input type="checkbox" v-model="options.base" v-on:change="updateOptions()" />
          <span class="text">Нужен ли цоколь</span>
          <span class="check"></span>
        </label>
      </div>
    </div>
    <div class="row">
      <div class="col col-xs-12">Длинна забора(м)
        <input class="input-length" type="number" min="0" v-model="options.length" v-on:change="updateOptions()" />
      </div>
    </div>                               
  </div>
</template>

<script>
import { EventBus } from '../main';
export default {
  data() {
    return {
      options: {
        sections: 3,
        peak: false,
        base: false,
        length: 0
      }
    }
  },
  methods: {
    updateOptions: function() {
      EventBus.$emit('ControlsUpdated', this.options)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.controls-switcher {
  background: rgba(255,255,255,0.8);
  padding: 10px;
  margin: 50px 0 0 20px;
  width: 300px;
  .col {
    font-size: 12px;
    text-transform: uppercase;
    color: #757575;
    height: 28px;
    line-height: 28px;
  }
}
.controls-checkbox {
  font-weight: normal;
  .text {
    line-height: 15px;
    height: 15px;
    display: inline-block;
    margin: 0 0.5rem 0 0;
  }
  input {
    display: none;
  }
  input:checked+.text+.check {
    background-image: url("./../assets/interface/tick.png");
    background-position: -2px -2px;
  }
  .check {
    display: inline-block;
    width: 16px;
    height: 16px;
    background: #f5f5f4;
    border: 1px #bbbcbe solid;
  }
}
.fence-height {
  font-weight: bold;
  color: #000;
  font-size: 14pt;
}
.input-length {
  display: inline-block;
  width: 50px;
  height: 24px;
}
</style>