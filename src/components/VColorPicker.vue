<template>
  <v-tabs dark
          show-arrows
          color="primary"
          slider-color="secondary"
          :class="{ 'hidden-bars': hideTabs }"
          v-model="activePaletteIndex">
    <v-tab :key="palette.key"
           @click="setPalleteIndex(index)"
           v-for="(palette, index) in palettes">
      {{palette.name}}
    </v-tab>
    <v-tab-item :key="palette.key"
                v-for="palette in palettes">
      <component :is="palette.component"
                 :value="value"
                 @input="setColor">
      </component>
    </v-tab-item>
  </v-tabs>
</template>

<script>
import VColorPaletteMaterial from './VColorPaletteMaterial';

export default {
  name: 'VColorPicker',
  components: {
    VColorPaletteMaterial,
  },
  props: {
    hideTabs: {
      type: Boolean,
      default: false,
    },
    value: {
      type: String,
    },
    returnType: {
      type: String,
      default: 'color',
    },
  },
  data() {
    return {
      activePaletteIndex: 0,
      color: null,
      palettes: [
        {
          key: 'material',
          name: 'Material',
          component: 'VColorPaletteMaterial',
        },
      ],
    };
  },
  methods: {
    getColorByReturnType() {
      return this.returnType === 'color' ? this.color : this.color[this.returnType];
    },
    setPalleteIndex(index) {
      this.activePaletteIndex = index;
    },
    setColor(value) {
      this.color = value;
      this.$emit('input', this.color.name);
      this.$emit('change', this.getColorByReturnType());
    },
  },
};
</script>

<style lang="stylus">
.v-tabs.hidden-bars .v-tabs__bar {
  display: none;
}
</style>
