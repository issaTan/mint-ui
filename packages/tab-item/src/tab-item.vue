<template>
  <a class="mint-tab-item"
    @click="handleClick(id)"
    :class="{ 'is-selected': $parent.value === id }">
    <div class="mint-tab-item-icon"><slot name="icon"></slot></div>
    <div class="mint-tab-item-label"><slot></slot></div>
  </a>
</template>

<script>
/**
 * mt-tab-item
 * @module components/tab-item
 * @desc 搭配 tabbar 或 navbar 使用
 * @param {*} id - 选中后的返回值，任意类型
 * @param {slot} [icon] - icon 图标
 * @param {slot} - 文字
 *
 * @example
 * <mt-tab-item>
 *   <img slot="icon" src="http://placehold.it/100x100">
 *   订单
 * </mt-tab-item>
 */
export default {
  name: 'mt-tab-item',

  props: ['id'],

  methods: {
    handleClick(id) {
      this.$parent.$emit('input', id);
      if (this.$Lazyload != null) {
        this.$Lazyload.lazyLoadHandler();
        this.$nextTick(() => {
          this.$Lazyload.lazyLoadHandler();
        });
      }
    }
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component tab-item {
      display: block;
      padding: 14px 0;
      flex: 1;
      text-decoration: none;

      @descendent icon {
        size: 54px;
        margin: 0 auto 10px;

        &:empty {
          display: none;
        }

        & > * {
          display: block;
          size: 100%;
        }
      }

      @descendent label {
        color: inherit;
        font-size: $tab-item-font-size;
        line-height: 1;
      }
    }
  }
</style>
