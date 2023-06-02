<template>
  <div class="menu-container">
    <mold v-if="moldEnable" :default-mold="defaultMold" @moldChange="handleMoldChange"/>
    <!--  <theme/>-->
    <arrange v-if="arrangeEnable"/>
    <styleOperation v-if="styleEnable"/>
    <fontOperation v-if="fontEnable"/>
  </div>

</template>

<script>
import mold from './mold'
import theme from './theme'
import arrange from './arrange'
import styleOperation from './styleOperation'
import fontOperation from './fontOperation.vue'
import {moleProps, viewMenuProps} from "../../../props";
import Locale from '/src/mixins/locale';

export default {
  name: 'viewMenu',
  mixins: [Locale],
  components: {
    mold,
    theme,
    arrange,
    styleOperation,
    fontOperation
  },
  props: {
    ...moleProps,
    ...viewMenuProps,
    minder: {}
  },
  methods: {
    handleMoldChange(data) {
      this.$emit('moldChange', data);
    }
  }
}

</script>

<style scoped>
.menu-container >>> i {
  display: inline-block;
  width: 20px;
  height: 20px;
}
</style>
