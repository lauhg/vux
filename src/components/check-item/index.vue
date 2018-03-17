<template>
  <div
  class="vux-cell-box weui-cell"
  :class="className"
  :style="style"
  @click="onClick"
  @contextmenu.prevent="onContextmenu">
    <icon :type="iconType" style="padding-right:1px;"></icon>
    <span>{{ item.text }}</span>
  </div>
</template>

<script>
import Icon from '../icon'

export default {
  name: 'check-item',
  components: {
    Icon
  },
  props: {
    isLink: Boolean,
    link: [String, Object],
    borderIntent: {
      type: Boolean,
      default: true
    },
    noFlex: Boolean,
    completedConfirm: {
      type: Boolean,
      default: true
    },
    checkItem: Object
  },
  data () {
    return {
      iconType: this.checkItem.completed ? 'success-circle' : 'circle',
      state: this.checkItem.completed ? 'completed' : 'todo',
      style: this.checkItem.completed ? {'color': '#d9d9d9', 'text-decoration': 'line-through'} : {},
      item: this.checkItem
    }
  },
  watch: {
    state (val) {
      let style = {}
      switch (val) {
        case 'completed':
          style['color'] = '#d9d9d9'
          style['text-decoration'] = 'line-through'
          this.style = style
          this.iconType = 'success-circle'
          break
        case 'todo':
          this.style = {}
          this.iconType = 'circle'
          break
        case 'ready':
          this.style = {}
          this.iconType = 'success'
          break
        default:
          break
      }
      this.$emit('completedToggle', this)
    },
    completedConfirm (confirm) {
      this.state = confirm ? 'completed' : 'todo'
    },
    checkItem: {
      handler (item) {
        this.item = item
        this.iconType = item.completed ? 'success-circle' : 'circle'
        this.state = item.completed ? 'completed' : 'todo'
        this.style = item.completed ? {'color': '#d9d9d9', 'text-decoration': 'line-through'} : {}
      },
      deep: true
    }
  },
  computed: {
    className () {
      return {
        'vux-tap-active': this.isLink || !!this.link,
        'weui-cell_access': this.isLink || !!this.link,
        'vux-cell-no-border-intent': !this.borderIntent
      }
    }
  },
  methods: {
    onClick () {
      switch (this.state) {
        case 'todo':
          this.state = 'ready'
          break
        case 'ready':
          this.state = 'todo'
          break
        case 'completed':
          return
        default:
          break
      }
    },
    onContextmenu (e) {
      this.$emit('longTap', this)
    }
  }
}
</script>

<style lang="less">
@import '../../styles/variable.less';
@import '../../styles/tap.less';
@import '../../styles/weui/base/mixin/setArrow.less';
@import '../../styles/weui/widget/weui_cell/weui_cell_global';

.vux-cell-primary {
  flex: 1;
}
.weui-cell.vux-cell-no-border-intent:before {
  left: 0;
}
</style>
