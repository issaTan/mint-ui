<template>
  <div class="mint-field"
       v-clickoutside="doCloseActive"
       :class="[{
      'is-textarea': type === 'textarea',
      'is-active': active || currentValue !== '',
    }]">
    <div class="mint-field-wrapper">
      <span class="mint-field-icon"><slot name="icon"></slot></span>
      <div class="mint-field-value">
        <textarea
            @change="$emit('change', currentValue)"
            ref="textarea"
            class="mint-field-core"
            :placeholder="placeholder"
            v-if="type === 'textarea'"
            :rows="rows"
            :disabled="disabled"
            :readonly="readonly"
            v-model="currentValue">
      </textarea>
        <input
            @change="$emit('change', currentValue)"
            ref="input"
            class="mint-field-core"
            :placeholder="placeholder"
            :number="type === 'number'"
            v-else
            :type="type"
            @focus="active = true"
            :disabled="disabled"
            :readonly="readonly"
            :value="currentValue"
            @input="handleInput">
      </div>
      <div
          @click="handleClear"
          class="mint-field-clear"
          v-if="!disableClear"
          v-show="currentValue && type !== 'textarea' && active">
        <i class="mintui mintui-field-error"></i>
      </div>
      <span class="mint-field-state" v-if="state" :class="['is-' + state]">
        <i class="mintui" :class="['mintui-field-' + state]"></i>
      </span>
    </div>
    <div class="mint-field-other">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import Clickoutside from 'ym-mint-ui/src/utils/clickoutside';
if (process.env.NODE_ENV === 'component') {
  require('ym-mint-ui/packages/cell/style.css');
}

/**
 * mt-field
 * @desc 编辑器，依赖 cell
 * @module components/field
 *
 * @param {string} [type=text] - field 类型，接受 text, textarea 等
 * @param {string} [label] - 标签
 * @param {string} [rows] - textarea 的 rows
 * @param {string} [placeholder] - placeholder
 * @param {string} [disabled] - disabled
 * @param {string} [readonly] - readonly
 * @param {string} [state] - 表单校验状态样式，接受 error, warning, success
 *
 * @example
 * <mt-field v-model="value" label="用户名"></mt-field>
 * <mt-field v-model="value" label="密码" placeholder="请输入密码"></mt-field>
 * <mt-field v-model="value" label="自我介绍" placeholder="自我介绍" type="textarea" rows="4"></mt-field>
 * <mt-field v-model="value" label="邮箱" placeholder="成功状态" state="success"></mt-field>
 */
export default {
  name: 'mt-field',

  data() {
    return {
      active: false,
      currentValue: this.value
    };
  },

  directives: {
    Clickoutside
  },

  props: {
    type: {
      type: String,
      default: 'text'
    },
    rows: String,
    placeholder: String,
    readonly: Boolean,
    disabled: Boolean,
    disableClear: Boolean,
    state: {
      type: String,
      default: 'default'
    },
    value: {},
    attr: Object
  },

  methods: {
    doCloseActive() {
      this.active = false;
    },

    handleInput(evt) {
      this.currentValue = evt.target.value;
    },

    handleClear() {
      if (this.disabled || this.readonly) return;
      this.currentValue = '';
    }
  },

  watch: {
    value(val) {
      this.currentValue = val;
    },

    currentValue(val) {
      this.$emit('input', val);
    },

    attr: {
      immediate: true,
      handler(attrs) {
        this.$nextTick(() => {
          const target = [this.$refs.input, this.$refs.textarea];
          target.forEach(el => {
            if (!el || !attrs) return;
            Object.keys(attrs).map(name => el.setAttribute(name, attrs[name]));
          });
        });
      }
    }
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component field {
      display: flex;
      height: 126px;
      background-color: $color-white;
      border-bottom: 1px solid $border-color;
      font-size: 30px;

      @when textarea {
        align-items: inherit;

        @descendent value {
          padding: 10px 0;
        }
      }

      @when active {
        background-color: $color-white;
      }

      @descendent wrapper {
        display: flex;
        flex: 1;
        align-items: center;
      }

      @descendent icon {
        color: $color-text-lighter;
        font-size: 40px;
        flex-shrik: 1;
        margin-right: 20px;
      }

      @descendent value {
        flex: 1;
      }

      @descendent core {
        appearance: none;
        border-radius: 0;
        border: 0;
        flex: 1;
        outline: 0;
        line-height: 1.6;
        font-size: inherit;
        width: 100%;
        background: inherit;
      }

      @descendent clear {
        opacity: .2;
      }

      @descendent state {
        color: inherit;
        margin-left: 30px;

        .mintui {
          font-size: 30px;
        }

        @when error {
          color: $error-color;
        }

        @when warning {
          color: $warning-color;
        }

        @when success {
          color: $success-color;
        }

        @when default {
          margin-left: 0;
        }
      }

      @descendent other {
        position: relative 0 0 * *;
      }
    }
  }
</style>
