<template lang="html">
  <header>
    <el-tabs v-model="activeName" class="mind_tab-content">
      <el-tab-pane :label="t('minder.main.header.minder')" name="editMenu">
        <div class="mind-tab-panel">
          <edit-menu
            :minder="minder"
            :move-enable="moveEnable"
            :sequence-enable="sequenceEnable"
            :tag-enable="tagEnable"
            :progress-enable="progressEnable"
            :priority-count="priorityCount"
            :priority-prefix="priorityPrefix"
            :tag-edit-check="tagEditCheck"
            :tag-disable-check="tagDisableCheck"
            :priority-disable-check="priorityDisableCheck"
            :priority-start-with-zero="priorityStartWithZero"
            :tags="tags"
            :move-confirm="moveConfirm"
            :distinct-tags="distinctTags"
            :del-confirm="delConfirm"
          />
        </div>
      </el-tab-pane>
      <el-tab-pane :label="t('minder.main.header.style')" name="viewMenu">
        <div class="mind-tab-panel">
          <view-menu
            v-if="viewMenuEnable"
            @moldChange="handleMoldChange"
            :minder="minder"
            :arrange-enable="arrangeEnable"
            :mold-enable="moldEnable"
            :font-enable="fontEnable"
            :style-enable="styleEnable"
            :default-mold="defaultMold"/>
        </div>
      </el-tab-pane>
    </el-tabs>
  </header>
</template>

<script>
  import editMenu from '../menu/edit/editMenu'
  import viewMenu from '../menu/view/viewMenu'
  import Locale from '/src/mixins/locale';
  import {delProps, editMenuProps, moleProps, priorityProps, tagProps, viewMenuProps} from "../../props";
  import mainEditor from "./mainEditor.vue";
  export default {
    name: 'headerVue',
    mixins: [Locale],
    data() {
      return {
        switchShow: {
          showEditMenu: true,
          showViewMenu: false,
        },
        activeName: 'editMenu'
      }
    },
    props: {
      ...editMenuProps,
      ...priorityProps,
      ...tagProps,
      ...moleProps,
      ...delProps,
      ...viewMenuProps,
      minder: {}
    },
    components: {
      mainEditor,
      editMenu,
      viewMenu
    },
    methods: {
      handleMoldChange(data) {
        this.$emit('moldChange', data);
      },
      showMenu: function (e) {
        for (var variable in this.switchShow) {
          if (this.switchShow.hasOwnProperty(variable)) {
            this.switchShow[variable] = false
          }
        }
        this['switchShow'][e.target.className.replace('btn-', '')] = true
      }
    }
  }
</script>

<style lang="scss" >
  @import "src/style/header";
</style>

<style scoped>

  .mind_tab-content >>> .tab-icons {
    background-image: url("../../assets/minder/icons.png");
    background-repeat: no-repeat;
  }

  .el-tabs >>> .el-tabs__header {
    margin-bottom: 10px;
  }


</style>
