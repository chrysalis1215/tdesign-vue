<template>
  <div>
    <!-- attach挂载 -->
    <t-button theme="primary" @click="visibleBody = true">挂载在body</t-button>
    <t-button theme="primary" @click="visibleIdAttach = true">挂载特定元素</t-button>
    <t-button theme="primary" @click="visibleFunctionAttach = true">挂载函数返回节点</t-button>

    <t-dialog
      :visible.sync="visibleBody"
      attach="body"
      header="挂载在body"
      destroyOnClose
      :onConfirm="()=>this.visibleBody = false"
    >
      <div slot="body">
        <div>被挂载到 body 元素的对话框</div>
      </div>
    </t-dialog>

    <t-dialog
      :visible.sync="visibleIdAttach"
      attach="#app"
      header="挂载到id为app的元素"
      destroyOnClose
      :onConfirm="()=>this.visibleIdAttach = false"
    >
      <div slot="body">
        <div>通过querySelect指定元素挂载</div>
        <div>支持原生document.querySelect选择元素</div>
        <div>querySelect获取到的第一个元素为挂载点</div>
      </div>
    </t-dialog>

    <t-dialog
      :visible.sync="visibleFunctionAttach"
      :attach="getAttach"
      header="函数返回挂载节点"
      destroyOnClose
      :onConfirm="()=>this.visibleFunctionAttach = false"
    >
      <div slot="body">
        <div>指定函数返回的节点为挂载点</div>
        <div>函数返回为DOM节点对象</div>
      </div>
    </t-dialog>

  </div>
</template>
<script>
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      visibleBody: false,
      visibleIdAttach: false,
      visibleFunctionAttach: false,
    };
  },
  methods: {
    getAttach() {
      return this.$root.$el;
    },

  },
});
</script>
<style scoped>
.t-button {
  margin-right: 20px;
}
</style>
