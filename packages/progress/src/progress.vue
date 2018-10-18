<template>
  <div class="mt-progress">
    <slot name="start"></slot>
    <div class="mt-progress-content">
      <div class="mt-progress-runway" :style="{ height: barHeight + 'px' }"></div>
      <div class="mt-progress-progress" :style="{ width: value + '%', height: barHeight + 'px' }"></div>
    </div>
    <slot name="end"></slot>
  </div>
</template>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mt {
    @component progress {
      position: relative;
      display: flex;
      height: 60px;
      line-height: 60px;

      & > * {
        display: flex;
        display: -webkit-box;
      }

      & *[slot="start"] {
        margin-right: 10px;
      }

      & *[slot="end"] {
        margin-left: 10px;
      }

      @descendent content {
        position: relative;
        flex: 1;
      }

      @descendent runway {
        position: absolute;
        transform: translate(0, -50%);
        top: 50%;
        left: 0;
        right: 0;
        background-color: #ebebeb;
        height: 6px;
      }

      @descendent progress {
        position: absolute;
        display: block;
        background-color: $color-primary;
        top: 50%;
        transform: translate(0, -50%);
        width: 0;
      }
    }
  }
</style>

<script type="text/ecmascript-6">
  export default {
    name: 'mt-progress',

    props: {
      value: Number,
      barHeight: {
        type: Number,
        default: 3
      }
    }
  };
</script>
