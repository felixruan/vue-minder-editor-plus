<template>
  <div>

    <el-select v-model="lang">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="$t(item.label)"
        :value="item.value">
      </el-option>
    </el-select>

    <minder-editor
      :import-json="importJson"
      :progress-enable="true"
      :tag-enable="true"
      :sequence-enable="true"
      :tags="tags"
      :move-enable="true"
      :move-confirm="moveConfirm"
      :disabled="false"
      :distinct-tags="distinctTags"
      :height="500"
      :priority-count="4"
      :tag-edit-check="test"
      :priority-disable-check="test"
      :priority-start-with-zero="true"
      @afterMount="afterMount()"
      :default-mold="3"
      :del-confirm="delConfirm"
      :style-enable="true"
      :arrange-enable="true"
      :mold-enable="true"
      :font-enable="true"
      @moldChange="handleMoldChange"
      @save="save"/>
  </div>
</template>

<script>

export default {
  name: "dev-test",
  data() {
    return {
      importJson: {
        // 节点数据
        root: {
          data: {
            // 文本内容
            text: 'vue-minder-editor-plus',
            // 标签
            resource: ['模块1'],
            // 是否禁止修改
            // disable: true,
            // 默认展开或折叠，默认是展开的，collapse 可设为折叠
            // expandState: 'collapse',
            // 在 disable 为 true 的情况下，允许添加标签
            // tagEnable: true,
            // 在 disable 为 true 的情况下，允许删除节点
            // allowDelete: true,
            // 在 disable 为 true 的情况下，允许添加标签，优先级比 tagEnable 高
            // allowDisabledTag: true,
          },
          // 子节点
          children: [
            {
              data: {
                text: 'child1',
                // disable: true,
                expandState: 'collapse',
                resource: ['模块2']
              },
              children: [
                {

                  "data": { "text": "地图axxaaaa",
                    disable: true,
                    allowDelete: true,
                    // tagEnable: true,
                    allowDisabledTag: true,
                    resource: ["模块12"],
                    priority: 3,
                    loaded: true
                  }
                }]
            },
            {
              data: {
                text: 'child2',
              }
            }
          ]
        },
      },
      tags:  ['模块1','模块2', '模块11'],
      distinctTags:  ['模块1','模块11'],
      options: [{
        value: 'en_US',
        label: 'en_US'
      }, {
        value: 'zh_CN',
        label: 'zh_CN'
      }, {
        value: 'zh_TW',
        label: 'zh_TW'
      }],
      lang: 'zh_CN'
    }
  },
  watch: {
    lang() {
      this.$setLang(this.lang);
    }
  },

  methods: {
    save(data) {
      console.log(data);
    },
    test() {
      return true;
    },
    handleMoldChange(a) {
      // console.log(a);
    },
    delConfirm() {
      console.log("-=-=-=-=-=-=-==");
      minder.forceRemoveNode();
    },
    afterMount() {
      // minder.on('selectionchange ', function (env) {
      //   console.log('selectionchange');
      //   console.log(env);
      //   // let selectNodes = env.minder.getSelectedNodes();
      //   // if (selectNodes) {
      //   //   selectNodes.forEach((node) => {
      //   //     markChangeNode(node);
      //   //   })
      //   // }
      // });
      minder.on('contentchange', function (env) {
        console.log('contentchange');
        // console.log(env);
        let selectNodes = env.minder.getSelectedNodes();
        console.log(selectNodes);
        console.log("=====");

      });
      // minder.on('afterExecCommand', function (env) {
      //   console.log('afterExecCommand');
      //   console.log(env);
      // });
      // minder.on('preExecCommand', function (env) {
      //   console.log('preExecCommand');
      //   console.log(env);
      // });
      minder.on('beforeExecCommand', function (env) {
        console.log('beforeExecCommand');
        console.log(env);
        return false;
      });

      this.addHotBox();

    },
    addHotBox() {

      let hotbox = window.minder.hotbox;
      let main = hotbox.state('main');
      main.button({
        position: 'ring',
        label: 'Test',
        key: '/',
        action: function () {
          alert("test")
        },
        enable: function () {
          return true;
        },
        beforeShow: function () {
        }
      })
    },
    moveConfirm() {
      return true;
    }
  }
}
</script>

<style scoped>

</style>
