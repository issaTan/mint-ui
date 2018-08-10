<script>
  export default {
    computed: {
      spinnerColor() {
        return this.color || this.$parent.color || '#eee';
      },

      spinnerSize() {
        return (this.size || this.$parent.size || 28) + 'px';
      }
    },

    props: {
      size: Number,
      color: String
    }
  };
</script>
