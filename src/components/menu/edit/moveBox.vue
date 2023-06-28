<template>
<div class="move-group ">
  <div class="move-up menu-btn" :disabled="arrangeUpDisabled" @click="execCommand('ArrangeUp')">
    <i class="tab-icons"></i>
    <span>{{t('minder.menu.move.up')}}</span>
  </div>
  <div class="move-down menu-btn" :disabled="arrangeDownDisabled" @click="execCommand('ArrangeDown')">
    <i class="tab-icons"></i>
    <span>{{t('minder.menu.move.down')}}</span>
  </div>
</div>
</template>

<script>
import {isDisableNode} from "../../../script/tool/utils";
import Locale from '/src/mixins/locale';
export default {
  name: 'moveBox',
  mixins: [Locale],
  props: ['moveEnable', 'moveConfirm'],
  data() {
    return {
      minder: undefined
    }
  },
  computed: {
    arrangeUpDisabled() {
      if (!this.moveEnable) return true;
      return this.isDisabled('ArrangeUp');
    },
    arrangeDownDisabled() {
      if (!this.moveEnable) return true;
      return this.isDisabled('ArrangeDown');
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.minder = minder;
      // 点击节点，触发computed
    })
  },
  methods: {
    execCommand(command) {
      if (command === 'ArrangeUp') {
        if (this.arrangeUpDisabled) {
          return;
        }
      } else if (command === 'ArrangeDown') {
        if (this.arrangeDownDisabled) {
          return;
        }
      }
      if (this.moveConfirm) {
        // 确认可以移动后，执行移动
        if (this.moveConfirm()) {
          minder.queryCommandState(command) === -1 || minder.execCommand(command)
        }
      } else {
        minder.queryCommandState(command) === -1 || minder.execCommand(command)
      }
    },
    isDisabled(command) {
      try {
        if (!this.minder) return false;
      } catch (e) {
        // 如果window的还没挂载minder，先捕捉undefined异常
        return false
      }
      if (isDisableNode(this.minder)) {
        return true;
      }
      if (minder && minder.queryCommandState) {
        return minder.queryCommandState(command) === -1;
      }
      return false;
    }
  }
}
</script>
