<!--
  switch 组件
  @param {Boolean} value 开关是否打开
  @param {String} [size=normal|large|small] 开关的大小
  @param {String} text 开关内部需要展示的文字,样式需要自己添加
  @param {Boolean} isDisabled 开关是否禁用
  @param {Object} appendClass 自定义class
  @param {Object} appendStyle 自定义Style对象
-->
<template>
  <div :style="appendStyle" :class="['vc-switch', appendClass, sizeClass, {'vc-switch-off': !value, 'vc-switch-disabled': isDisabled}]" @click="toggleSwitch">
    {{text}}
    <i></i>
  </div>
</template>

<script>
  import {componentBaseParamConfig} from '../base-config';

  export default {
    props: Object.assign({}, componentBaseParamConfig, {
      size: {
        type: String,
        default: 'normal'
      },
      value: {
        type: Boolean,
        default: false
      },
      isDisabled: {
        type: Boolean,
        default: false
      },
      text: {
        type: String,
        default: ''
      }
    }),

  data () {
    return {};
  },

  computed: {
    sizeClass() {
      const sizeMap = {
        'normal': '',
        'small': 'vc-switch-sm'
      };

      return sizeMap[this.size]
    }
  },

  methods: {
    toggleSwitch() {
      if(this.isDisabled) return;

      this.value = !this.value;
    }
  }

  }
</script>

<style lang="scss">
  @import "style.scss";
</style>