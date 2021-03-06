<template>
  <div class="mint-radiolist" @change="$emit('change', currentValue)">
    <label class="mint-radiolist-title" v-text="title"></label>
    <x-cell v-for="option in options">
      <label class="mint-radiolist-label" slot="title">
        <span
          :class="{'is-right': align === 'right'}"
          class="mint-radio">
          <input
            class="mint-radio-input"
            type="radio"
            v-model="currentValue"
            :disabled="option.disabled"
            :value="option.value || option">
          <span class="mint-radio-core"></span>
        </span>
        <span class="mint-radio-label" v-text="option.label || option"></span>
      </label>
    </x-cell>
  </div>
</template>

<script>
import XCell from 'ym-mint-ui/packages/cell/index.js';
if (process.env.NODE_ENV === 'component') {
  require('ym-mint-ui/packages/cell/style.css');
}
/**
 * mt-radio
 * @module components/radio
 * @desc 单选框列表，依赖 cell 组件
 *
 * @param {string[], object[]} options - 选项数组，可以传入 [{label: 'label', value: 'value', disabled: true}] 或者 ['ab', 'cd', 'ef']
 * @param {string} value - 选中值
 * @param {string} title - 标题
 * @param {string} [align=left] - checkbox 对齐位置，`left`, `right`
 *
 * @example
 * <mt-radio v-model="value" :options="['a', 'b', 'c']"></mt-radio>
 */
export default {
  name: 'mt-radio',

  props: {
    title: String,
    align: String,
    options: {
      type: Array,
      required: true
    },
    value: String
  },

  data() {
    return {
      currentValue: this.value
    };
  },

  watch: {
    value(val) {
      this.currentValue = val;
    },

    currentValue(val) {
      this.$emit('input', val);
    }
  },

  components: {
    XCell
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component radiolist {

      .mint-cell {
        padding: 0;
      }

      @descendent label {
        display: block;
        padding: 0 20px;
      }

      @descendent title {
        font-size: 24px;
        margin: 16px;
        display: block;
        color: $radio-title-color;
      }
    }

    @component radio {
      @when right {
        float: right;
      }

      @descendent label {
        vertical-align: middle;
        margin-left: 12px;
      }

      @descendent input {
        display: none;

        &:checked {
          + .mint-radio-core {
            background-color: $color-primary;
            border-color: $color-primary;

            &::after {
              background-color: $color-white;
              transform: scale(1);
            }
          }
        }

        &[disabled] + .mint-radio-core {
          background-color: $border-color;
          border-color: $border-color;
        }
      }

      @descendent core {
        box-sizing: border-box;
        display: inline-block;
        background-color: $color-white;
        border-radius: 100%;
        border: 1px solid #ccc;
        position: relative;
        size: 40px;
        vertical-align: middle;

        &::after {
          content: " " !important;
          border-radius: 100%;
          position: absolute 10px * * 10px;
          size: 16px;
          transition: transform .2s;
          transform: scale(0);
        }
      }
    }
  }
</style>
