<!--
  select 下拉列表
  @param {Object} data 传入组件数据,用于渲染
    ex:
        {
          optsList: [{
            value: 0,
            label: 'value0'
          }, {
            value: 1,
            label: 'value1'
            // 特性渲染函数
            render: function() {
                return <a>111</a>
            }
          }, {
            value: 2,
            label: 'value2',
            isDisabled: true
          }]
        }
  @param {String | Number} value 绑定外部数据对象的结果
  @param {Boolean} isDisabled 当前下拉列表是否可选
  @param {Boolean} isOpened 下拉列表是否显示
  @param {Function} onOpen 下拉列表展示时回调
  @param {Boolean} isEditAble 下拉列表是否可编辑,如果可编辑,则展示框变成输入框,并自带过滤功能
  @param {Function} onSelect 选择事件
  @param {Object} appendClass 自定义class
  @param {Object} appendStyle 自定义Style对象
-->
<template>
    <pv-dropdown :data="data"
                 :append-style="appendStyle"
                 :append-class="appendClass"
                 :value.sync="value"
                 :is-disabled="isDisabled"
                 :on-select="onSelected"
                 :is-opened.sync="isOpened"
                 :as-label="asLabel"
                 :as-value="asValue"
                 :filter="isEditAble ? inputSelect : ''">
        <span class="vc-select-selection vc-select-selection-single" @click.stop="toggle">
        <pv-input v-if="isEditAble && !isDisabled" v-model="inputSelect" ></pv-input>
        <span v-if="isDisabled || !isEditAble" class="vc-select-selection-rendered">{{currentSelected}}</span>
        <span class="vc-select-arrow"></span>
        </span>
    </pv-dropdown>
</template>

<script>
    import {componentBaseParamConfig, alias} from '../base-config';
    import pvDropdown from '../dropdown';
    import pvInput from '../input';

    export default {
        props: Object.assign({}, componentBaseParamConfig, alias, {
            isDisabled: {
                type: Boolean,
                default: false
            },
            data: {
                type: Object,
                default: function() {
                    return {
                        optsList: []
                    }
                }
            },
            value: {
            },
            onSelect: {
                type: Function
            },
            isOpened: {
                type: Boolean,
                default: false
            },
            isEditAble: {
                type: Boolean,
                default: false
            },
            onOpen: {
                type: Function
            }
        }),

        data () {
            return {
                inputSelect: ''
            }
        },

        computed: {
            currentSelected() {
                const _this = this;
                let res = _this.data.optsList.find(function(it) {
                    return it.value == _this.value;
                });

                let returnValue = '请选择';

                if(!res) return returnValue;

                if(res.label !== undefined || res[this.asLabel] !== undefined) {
                    returnValue = res.label || res[this.asLabel];
                } else if(res.value !== undefined || res[this.asValue] !== undefined) {
                    returnValue = res.value || res[this.asValue];
                }

                return returnValue;
            }
        },

        methods: {
            onSelected(index, item) {
                let opts = this.data.optsList;

                if(opts.isDisabled) return;

                this.inputSelect = this.currentSelected = opts[index].label || this.value;
                this.onSelect && this.onSelect(item.value, index);
            },

            toggle() {
                if(this.isDisabled) return;

                this.isOpened = !this.isOpened;

                if(this.isOpened) this.onOpen && this.onOpen();
            }
        },

        ready() {
            // 初始化input数据
            this.inputSelect = this.currentSelected;
        },

        watch: {
            // 数据变化重置初始值
            data() {
                this.inputSelect = this.currentSelected;
            }
        },

        components: {
            pvDropdown,
            pvInput
        }
    }
</script>

<!-- 这里控制了 dropdown 所以不能加上scoped -->
<style lang="scss">
    @import "style.scss";
</style>