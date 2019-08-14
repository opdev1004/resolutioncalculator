<template>
  <div id="ColorPicker" class="ColorPicker">
    <div id="EmptySpace" class="EmptySpace"></div>
    <div id="CPWrapper" class="CPWrapper">
      <div id="CPComponents" class="CPComponents" :style="CPStyle">
        <Slider v-model="color" />
        <Swatches v-model="color" />
        <Chrome v-model="color" />
        <Material v-model="color" />
        <Grayscale v-model="color" />
      </div>
    </div>
  </div>
</template>

<script>
import { Slider, Swatches, Chrome, Material, Grayscale } from 'vue-color';

export default {
  name: 'ColorPicker',
  components: {
    Slider,
    Swatches,
    Chrome,
    Material,
    Grayscale
  },
  data: function() {
    return {
      color: {
        hex: '#23272f',
        hsl: { h: 220, s: 0.146, l: 0.161, a: 1 },
        hsv: { h: 220, s: 0.26, v: 0.18, a: 1 },
        rgba: { r: 35, g: 39, b: 47, a: 1 },
        a: 1
      }
    }
  },
  computed: {
    CPStyle(){
      return {
        'background-color' : this.color.hex
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#ColorPicker {
  min-height: 100%;
}

#CPWrapper {
  margin: 10px;
  min-height: calc(100% - 22px);
}

#CPComponents{
  margin: 10px;
  padding: 10px;
  min-height: calc(100% - 40px);
}

.vc-swatches {
  width: 100% !important;
  height: 160px !important;
}

.vc-slider {
  width: calc(100% - 40px);
  margin: 10px auto 20px auto;
  padding: 20px;
  background-color: #ffffff;
}

.vc-chrome,
.vc-material,
.vc-grayscale {
  margin: 20px 20px 0px 0px;
  display: inline-block;
  vertical-align: top;
}

</style>
