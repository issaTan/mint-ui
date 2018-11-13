<template>
  <div class="mt-range" :class="{ 'mt-range--disabled': disabled }">
    <slot name="start"></slot>
    <div class="mt-range-content" ref="content" @click="handleClick">
      <div class="mt-range-runway" :style="{ 'border-top-width': barHeight + 'px' }"></div>
      <div class="mt-range-progress" :style="{ width: progress + '%', height: barHeight + 'px' }"></div>
      <div class="mt-range-thumb" ref="thumb" :style="{ left: progress + '%' }">
        <slot name="thumb"></slot>
      </div>
    </div>
    <slot name="end"></slot>
  </div>
</template>

<style lang="css">
  @import "../../../src/style/var.css";

  @component-namespace mt {
    @component range {
      position: relative;
      display: flex;
      height: $range-thumb-size;
      line-height: $range-thumb-size;

      & > * {
        display: flex;
        display: -webkit-box;
      }

      & *[slot=start] {
        margin-right: 10px;
      }

      & *[slot=end] {
        margin-left: 10px;
      }

      @descendent content {
        position: relative;
        flex: 1;
        margin-right: $range-thumb-size;
      }

      @descendent runway {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        left: 0;
        right: -$range-thumb-size;
        border-top-color: $range-background-color;
        border-top-style: solid;
        border-radius: 3px;
      }

      @descendent thumb {
        background-color: $color-white;
        position: absolute;
        left: 0;
        top: 0;
        width: $range-thumb-size;
        height: $range-thumb-size;
        border-radius: 100%;
        cursor: move;
        box-shadow:0 0 5px 0 rgba(40, 40, 40, 0.25);
      }

      @descendent progress {
        position: absolute;
        display: block;
        background-color: $range-background-primary-color;
        top: 50%;
        transform: translateY(-50%);
        width: 0;
        border-radius: 3px;
      }

    }
  }
</style>

<script type="text/babel">
  import draggable from './draggable';

  export default {
    name: 'mt-range',

    props: {
      min: {
        type: Number,
        default: 0
      },
      max: {
        type: Number,
        default: 100
      },
      step: {
        type: Number,
        default: 1
      },
      disabled: {
        type: Boolean,
        default: false
      },
      value: {
        type: Number
      },
      barHeight: {
        type: Number,
        default: 1
      }
    },

    computed: {
      progress() {
        const value = this.value;
        if (typeof value === 'undefined' || value === null) return 0;
        return Math.floor((value - this.min) / (this.max - this.min) * 100);
      }
    },

    mounted() {
      const thumb = this.$refs.thumb;
      const content = this.$refs.content;

      const getThumbPosition = () => {
        const contentBox = content.getBoundingClientRect();
        const thumbBox = thumb.getBoundingClientRect();
        return {
          left: thumbBox.left - contentBox.left,
          top: thumbBox.top - contentBox.top,
          thumbBoxLeft: thumbBox.left
        };
      };

      let dragState = {};
      draggable(thumb, {
        start: (event) => {
          if (this.disabled) return;
          this.$emit('start');
          const position = getThumbPosition();
          const thumbClickDetalX = event.clientX - position.thumbBoxLeft;
          dragState = {
            thumbStartLeft: position.left,
            thumbStartTop: position.top,
            thumbClickDetalX: thumbClickDetalX
          };
        },
        drag: (event) => {
          if (this.disabled) return;
          const contentBox = content.getBoundingClientRect();
          const deltaX = event.pageX - contentBox.left - dragState.thumbStartLeft - dragState.thumbClickDetalX;
          const stepCount = Math.ceil((this.max - this.min) / this.step);
          const newPosition = (dragState.thumbStartLeft + deltaX) - (dragState.thumbStartLeft + deltaX) % (contentBox.width / stepCount);

          let newProgress = newPosition / contentBox.width;

          if (newProgress < 0) {
            newProgress = 0;
          } else if (newProgress > 1) {
            newProgress = 1;
          }

          this.$emit('input', Math.round(this.min + newProgress * (this.max - this.min)));
        },
        end: () => {
          if (this.disabled) return;
          this.$emit('change', this.value);
          dragState = {};
        }
      });
    },
    methods: {
      handleClick($event) {
        if (this.disabled) return;
        const content = this.$refs.content;
        const contentBox = content.getBoundingClientRect();
        const clickLength = $event.clientX - contentBox.left;
        this.$emit('click', clickLength / contentBox.width);
      }
    }
  };
</script>
