<template>
  <a class="mint-cell" :href="href">
    <span class="mint-cell-mask" v-if="isLink"></span>
    <div class="mint-cell-left">
      <slot name="left"></slot>
    </div>
    <div class="mint-cell-wrapper">
      <div class="mint-cell-title">
        <slot name="icon">
          <i v-if="icon" class="mintui" :class="'mintui-' + icon"></i>
        </slot>
        <slot name="title">
          <span class="mint-cell-text" v-text="title"></span>
          <span v-if="label" class="mint-cell-label" v-text="label"></span>
        </slot>
      </div>
      <div class="mint-cell-value" :class="{ 'is-link' : isLink }">
        <slot>
          <span v-text="value"></span>
        </slot>
      </div>
      <span class="mint-cell-right-icon" v-if="isLink">
        <svg t="1539767082063" class="icon" style="fill: #999;" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2745" xmlns:xlink="http://www.w3.org/1999/xlink" width="100%" height="100%"><path d="M761.056 532.128c0.512-0.992 1.344-1.824 1.792-2.848 8.8-18.304 5.92-40.704-9.664-55.424L399.936 139.744c-19.264-18.208-49.632-17.344-67.872 1.888-18.208 19.264-17.376 49.632 1.888 67.872l316.96 299.84-315.712 304.288c-19.072 18.4-19.648 48.768-1.248 67.872 9.408 9.792 21.984 14.688 34.56 14.688 12 0 24-4.48 33.312-13.44l350.048-337.376c0.672-0.672 0.928-1.6 1.6-2.304 0.512-0.48 1.056-0.832 1.568-1.344C757.76 538.88 759.2 535.392 761.056 532.128z" p-id="2746"></path></svg>
      </span>
    </div>
    <div class="mint-cell-right">
      <slot name="right"></slot>
    </div>
  </a>
</template>

<script>
if (process.env.NODE_ENV === 'component') {
  require('ym-mint-ui/packages/font/style.css');
}

/**
 * mt-cell
 * @module components/cell
 * @desc 单元格
 * @param {string|Object} [to] - 跳转链接，使用 vue-router 的情况下 to 会传递给 router.push，否则作为 a 标签的 href 属性处理
 * @param {string} [icon] - 图标，提供 more, back，或者自定义的图标（传入不带前缀的图标类名，最后拼接成 .mintui-xxx）
 * @param {string} [title] - 标题
 * @param {string} [label] - 备注信息
 * @param {boolean} [is-link=false] - 可点击的链接
 * @param {string} [value] - 右侧显示文字
 * @param {slot} - 同 value, 会覆盖 value 属性
 * @param {slot} [title] - 同 title, 会覆盖 title 属性
 * @param {slot} [icon] - 同 icon, 会覆盖 icon 属性，例如可以传入图片
 *
 * @example
 * <mt-cell title="标题文字" icon="back" is-link value="描述文字"></mt-cell>
 * <mt-cell title="标题文字" icon="back">
 *   <div slot="value">描述文字啊哈</div>
 * </mt-cell>
 */
export default {
  name: 'mt-cell',

  props: {
    to: [String, Object],
    icon: String,
    title: String,
    label: String,
    isLink: Boolean,
    value: {}
  },

  computed: {
    href() {
      if (this.to && !this.added && this.$router) {
        const resolved = this.$router.match(this.to);
        if (!resolved.matched.length) return this.to;

        this.$nextTick(() => {
          this.added = true;
          this.$el.addEventListener('click', this.handleClick);
        });
        return resolved.fullPath || resolved.path;
      }
      return this.to;
    }
  },

  methods: {
    handleClick($event) {
      $event.preventDefault();
      this.$router.push(this.href);
    }
  }
};
</script>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mint {
    @component cell {
      background-color: $color-white;
      box-sizing: border-box;
      color: inherit;
      min-height: 110px;
      display: block;
      overflow: hidden;
      position: relative;
      text-decoration: none;

      &:first-child {
        .mint-cell-wrapper {
          background-origin: border-box;
        }
      }

      &:last-child {
        background-image: linear-gradient(0deg, $border-color, $border-color 50%, transparent 50%);
        background-size: 100% 2px;
        background-repeat: no-repeat;
        background-position: bottom;
      }

      @descendent wrapper {
        background-image:linear-gradient(180deg, $border-color, $border-color 50%, transparent 50%);
        background-size: 120% 2px;
        background-repeat: no-repeat;
        background-position: top left;
        background-origin: content-box;
        align-items: center;
        box-sizing: border-box;
        display: flex;
        font-size: 28px;
        line-height: 1;
        min-height: inherit;
        overflow: hidden;
        padding: 0 30px;
        width: 100%;
      }

      @descendent mask {
        &::after {
          background-color: #000;
          content: " " !important;
          opacity: 0;
          position: absolute 0;
        }

        &:active::after {
          opacity: .1;
        }
      }

      @descendent text {
        vertical-align: middle;
      }

      @descendent label {
        color: #999;
        display: block;
        font-size: 24px;
        margin-top: 12px;
      }

      img {
        vertical-align: middle;
      }

      @descendent title {
        flex: 1;
      }

      @descendent value {
        color: $cell-value-color;
        display: flex;
        align-items: center;

        @when link {
          margin-right: 48px;
        }
      }

      @descendent left {
        position: absolute;
        height: 100%;
        left: 0;
        transform: translate3d(-100%, 0, 0);
      }

      @descendent right {
        position: absolute;
        height: 100%;
        right: 0;
        top: 0;
        transform: translate3d(100%, 0, 0);
      }

      @descendent right-icon {
        width: 36px;
        height: 36px;
      }
    }
  }
</style>
