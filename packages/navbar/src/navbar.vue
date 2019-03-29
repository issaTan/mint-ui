<template>
  <div class="mint-navbar" :class="{ 'is-fixed': fixed }">
    <slot></slot>
  </div>
</template>

<script>
/**
 * mt-navbar
 * @module components/navbar
 * @desc 顶部 tab，依赖 tab-item
 *
 * @param {boolean} [fixed=false] - 固定底部
 * @param {*} selected - 返回 item component 传入的 value
 *
 * @example
 * <mt-navbar :selected.sync="selected">
 *   <mt-tab-item value="订单">
 *     <span slot="label">订单</span>
 *   </mt-tab-item>
 * </mt-navbar>
 *
 * <mt-navbar :selected.sync="selected" fixed>
 *   <mt-tab-item :value="['传入数组', '也是可以的']">
 *     <span slot="label">订单</span>
 *   </mt-tab-item>
 * </mt-navbar>
 *
 */
export default {
  name: 'mt-navbar',

  props: {
    fixed: Boolean,
    value: {}
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component navbar {
      color: $color-text-light;
      background-color: $color-white;
      display: flex;
      text-align: center;

      @when fixed {
        position: fixed 0 0 * 0;
        z-index: $z-index-normal;
      }

      .mint-tab-item {
        padding: 30px 0;
        font-size: 32px;
        position: relative;
        border-bottom: 1px solid $border-color;

        &::before, &::after {
         content: " " !important;
         display: block;
        }

        &::before {
          width: 60px;
          height: 6px;
          background-color: $color-white;
          border-radius: 3px;
          position: absolute;
          bottom: 0;
          left: 50%;
          transform: translateX(-50%);
        }

        &::after {
          width: 1px;
          height: 30px;
          background-color: $border-color;
          position: absolute;
          right: 0;
          top: 50%;
          transform: translateY(-50%);
         }

        &:last-child {
          &::after {
            width: 0;
          }
        }

        &.is-selected {
          font-weight: bold;
          color: $color-text-primary;
          &::before {
            background-color: $color-text-primary;
          }
        }
      }
    }
  }
</style>
