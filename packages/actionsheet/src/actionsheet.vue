<template>
  <transition name="actionsheet-float">
    <div v-show="currentValue" class="mint-actionsheet">
      <ul class="mint-actionsheet-list" :style="{ 'margin-bottom': cancelText ? '10px' : '0' }">
        <li v-for="( item, index ) in actions" class="mint-actionsheet-listitem" @click.stop="itemClick(item, index)">{{ item.name }}</li>
      </ul>
      <a class="mint-actionsheet-button" @click.stop="currentValue = false" v-if="cancelText">{{ cancelText }}</a>
    </div>
  </transition>
</template>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component actionsheet {
      position: fixed;
      background: #e0e0e0;
      width: 100%;
      text-align: center;
      bottom: 0;
      left: 50%;
      max-height: 100%;
      overflow-y: auto;
      transform: translate3d(-50%, 0, 0);
      backface-visibility: hidden;
      transition: transform .3s ease-out;

      @descendent list {
        list-style: none;
        padding: 0;
        margin: 0;
      }

      @descendent listitem {
        border-bottom: solid 1px #e0e0e0;
      }

      @descendent listitem, button {
        display: block;
        width: 100%;
        height: 90px;
        line-height: 90px;
        font-size: 36px;
        color: $color-base;
        background-color: $color-white;
        &:active {
           background-color: #f0f0f0;
        }
      }
    }
  }

  .actionsheet-float-enter,
  .actionsheet-float-leave-active {
    transform: translate3d(-50%, 100%, 0);
  }
</style>

<script type="text/babel">
  import Popup from 'ym-mint-ui/src/utils/popup';
  import 'ym-mint-ui/src/style/popup.css';

  export default {
    name: 'mt-actionsheet',

    mixins: [Popup],

    props: {
      modal: {
        default: true
      },

      modalFade: {
        default: false
      },

      lockScroll: {
        default: false
      },

      closeOnClickModal: {
        default: true
      },

      cancelText: {
        type: String,
        default: '取消'
      },

      actions: {
        type: Array,
        default: () => []
      }
    },

    data() {
      return {
        currentValue: false
      };
    },

    watch: {
      currentValue(val) {
        this.$emit('input', val);
      },

      value(val) {
        this.currentValue = val;
      }
    },

    methods: {
      itemClick(item, index) {
        if (item.method && typeof item.method === 'function') {
          item.method(item, index);
        }
        this.currentValue = false;
      }
    },

    mounted() {
      if (this.value) {
        this.rendered = true;
        this.currentValue = true;
        this.open();
      }
    }
  };
</script>
