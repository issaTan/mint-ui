<template>
  <transition name="mint-toast-pop">
    <div class="mint-toast" v-show="visible" :class="customClass" :style="{ 'padding': iconClass === '' ? '5px 12px' : '10px' }">
      <i class="mint-toast-icon" :class="iconClass" v-if="iconClass !== ''"></i>
      <span class="mint-toast-text" :style="{ 'padding-top': iconClass === '' ? '0' : '10px' }">{{ message }}</span>
    </div>
  </transition>
</template>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component toast {
      position: fixed;
      max-width: 80%;
      border-radius: 15px;
      background: rgba(0, 0, 0, 0.7);
      color: $color-white;
      box-sizing: border-box;
      text-align: center;
      z-index: 10000;
      transition: opacity .3s linear;
  
      @descendent icon {
        display: block;
        text-align: center;
        font-size: 112px;
      }
      
      @descendent text {
        font-size: 28px;
        display: block;
        text-align: center;
      }
      
      @when placetop {
        top: 100px;
        left: 50%;
        transform: translate(-50%, 0);
      }
      
      @when placemiddle {
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
      }
      
      @when placebottom {
        bottom: 100px;
        left: 50%;
        transform: translate(-50%, 0);
      }
      
      @descendent pop-enter, pop-leave-active {
        opacity: 0;
      }
    }
  }
</style>

<script type="text/babel">
  export default {
    props: {
      message: String,
      className: {
        type: String,
        default: ''
      },
      position: {
        type: String,
        default: 'middle'
      },
      iconClass: {
        type: String,
        default: ''
      }
    },

    data() {
      return {
        visible: false
      };
    },

    computed: {
      customClass() {
        var classes = [];
        switch (this.position) {
          case 'top':
            classes.push('is-placetop');
            break;
          case 'bottom':
            classes.push('is-placebottom');
            break;
          default:
            classes.push('is-placemiddle');
        }
        classes.push(this.className);

        return classes.join(' ');
      }
    }
  };
</script>