<template>
  <button
    :type="nativeType"
    class="mint-button"
    :class="['mint-button--' + type, 'mint-button--' + size, {
      'is-disabled': disabled,
      'is-plain': plain
    }]"
    @click="handleClick"
    :disabled="disabled">
    <span class="mint-button-icon" v-if="icon || $slots.icon">
      <slot name="icon">
        <i v-if="icon" class="mintui" :class="'mintui-' + icon"></i>
      </slot>
    </span>
    <label class="mint-button-text"><slot></slot></label>
  </button>
</template>

<script>
if (process.env.NODE_ENV === 'component') {
  require('ym-mint-ui/packages/font/style.css');
}

/**
 * mt-header
 * @module components/button
 * @desc 按钮
 * @param {string} [type=default] - 显示类型，接受 default, primary, danger
 * @param {boolean} [disabled=false] - 禁用
 * @param {boolean} [plain=false] - 幽灵按钮
 * @param {string} [size=normal] - 尺寸，接受 normal, small, large
 * @param {string} [native-type] - 原生 type 属性
 * @param {string} [icon] - 图标，提供 more, back，或者自定义的图标（传入不带前缀的图标类名，最后拼接成 .mintui-xxx）
 * @param {slot} - 显示文本
 * @param {slot} [icon] 显示图标
 *
 * @example
 * <mt-button size="large" icon="back" type="primary">按钮</mt-button>
 */
export default {
  name: 'mt-button',

  methods: {
    handleClick(evt) {
      this.$emit('click', evt);
    }
  },

  props: {
    icon: String,
    disabled: Boolean,
    nativeType: String,
    plain: Boolean,
    type: {
      type: String,
      default: 'default',
      validator(value) {
        return [
          'default',
          'danger',
          'primary',
          'text'
        ].indexOf(value) > -1;
      }
    },
    size: {
      type: String,
      default: 'normal',
      validator(value) {
        return [
          'extra-extra-small',
          'extra-small',
          'small',
          'normal',
          'medium',
          'large'
        ].indexOf(value) > -1;
      }
    }
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component button {
      appearance: none;
      border-radius: 4px;
      border: 0;
      box-sizing: border-box;
      color: inherit;
      display: block;
      font-size: 28px;
      height: 60px;
      outline: 0;
      overflow: hidden;
      position: relative;
      text-align: center;

      &::after {
        background-color: rgba(0, 0, 0, .4);
        content: " " !important;
        opacity: 0;
        position: absolute 0 0 0 0;
      }

      &:not(.is-disabled):active::after {
        opacity: 1;
      }

      @descendent icon {
        vertical-align: middle;
        display: inline-block;
      }

      @descendent text {
        line-height: 1;
      }

      @modifier text {
        color: inherit;
        font-size: inherit;
        background-color: transparent;
        box-shadow: none;
        height: auto;
      }

      @modifier default {
        color: $button-default-color;
        background-color: $button-default-background-color;
        box-shadow: $button-default-box-shadow;

        @when plain {
          border: 1px solid $button-default-plain-color;
          background-color: transparent;
          box-shadow: none;
          color: $button-default-plain-color;
        }
      }

      @modifier primary {
        color: $button-primary-color;
        background-color: $button-primary-background-color;

        @when plain {
          border: 1px solid $button-primary-background-color;
          background-color: transparent;
          color: $button-primary-background-color;
        }
      }

      @modifier danger {
        color: $button-danger-color;
        background-color: $button-danger-background-color;

        @when plain {
          border: 1px solid $button-danger-background-color;
          background-color: transparent;
          color: $button-danger-background-color;
        }
      }

      @modifier extra-extra-small {
        display: inline-block;
        font-size: 24px;
        padding: 0 24px;
        height: 40px;
        @descendent text {
          vertical-align: middle;
        }
      }

      @modifier extra-small {
        display: inline-block;
        font-size: 28px;
        padding: 0 24px;
        height: 50px;
        @descendent text {
          vertical-align: middle;
        }
      }

      @modifier small {
        display: inline-block;
        font-size: 28px;
        padding: 0 24px;
        height: 60px;
        @descendent text {
          vertical-align: middle;
        }
      }

      @modifier normal {
        display: inline-block;
        font-size: 28px;
        padding: 0 42px;
        height: 70px;
        @descendent text {
          vertical-align: middle;
        }
      }

      @modifier medium {
        display: inline-block;
        font-size: 32px;
        height: 90px;
        width: 100%;
        @descendent text {
          vertical-align: middle;
        }
      }

      @modifier large {
        font-size: 32px;
        display: block;
        height: 100px;
        width: 100%;
      }

      @when disabled {
        opacity: .5;
      }
    }
  }
</style>
