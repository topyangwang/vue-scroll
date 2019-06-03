<template>
  <div
    :ref="'scrollContainer'+id"
    class="scroll_container"
    @mouseover="stopScroll"
    @mouseout="scroll"
  >
    <div :ref="'scrollBox'+id" :class="{anim: animate}">
      <ul>
          <li class="roll_item" v-for="(item, k) in rollDataFragment" :key="item.indexKey || k">
            <slot class="scroll_content" :item = item :index = k></slot>
          </li>
        </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    rollData: {// 必须提供字段
      required: false,
    },
    rollHeight: {// 可选字段，有默认值
      required: true,
    },
    interval: {
      default: 2000,
    },
    id: {
      default: 0,
    },
    rollType: {
      default: 'data',
      validator: function (value) {// eslint-disable-line
        // 这个值必须匹配下列字符串中的一个
        return ['dom', 'data'].indexOf(value) !== -1;
      },
    },
    roll: {
      default: true,
    },
    indexKey: {
      default: '',
    },
  },
  data() {
    return {
      animate: false,
      scrollTimer: null,
      isOK: true,
      showAmount: 0, // 可展示行数
    };
  },
  created() {
    if (this.rollType === 'data' && !this.rollData) {
      console.error('此循环模式rollData是必须的');// eslint-disable-line
      this.isOK = false;
      return;
    }
    this.getHeight();
    window.onresize = this.getHeight;
    this.scroll();
  },
  computed: {
    rollDataFragment() { // 用于渲染的数据
      if (this.rollType === 'data') {
        return this.rollData.slice(0, Math.ceil(this.showAmount) + 1);
      } else {// eslint-disable-line
        return this.rollData;
      }
    },
  },
  beforeDestroy() {
    this.stopScroll();
  },
  methods: {
    scroll() {
      if (this.scrollTimer === null && this.isOK) {
        let scrollDom;
        this.scrollTimer = setInterval(() => {
          if (!this.roll) { // 滚动设置为false停止滚动
            this.stopScroll();
            return;
          }
          // 滚过显示框高度大于所有数据高度停止滚动
          if ((this.rollDataFragment.length === this.rollData.length) && (this.showAmount > this.rollData.length)) {// eslint-disable-line
            this.stopScroll();
            return;
          }
          // item框
          const scrollBox = this.$refs['scrollBox' + this.id];// eslint-disable-line
          this.rollType === 'data' && this.rollData.push(this.rollData[0]);// eslint-disable-line
          this.rollType === 'dom' && (scrollDom = scrollBox.getElementsByClassName('roll_item'));// eslint-disable-line
          scrollBox.style.marginTop = '-' + this.rollHeight;// eslint-disable-line
          this.animate = !this.animate;
          setTimeout(() => {
            this.rollType === 'data' && this.rollData.shift();// eslint-disable-line
            this.rollType === 'dom' && scrollDom[0].parentNode.appendChild(scrollDom[0]);// eslint-disable-line
            scrollBox.style.marginTop = '0px';
            this.animate = !this.animate;
          }, 500);
        }, this.interval);
      }
    },
    stopScroll() { // 停止滚动
      clearInterval(this.scrollTimer);
      this.scrollTimer = null;
    },
    getHeight() { // 获取滚动框的高度并计算可显示行数
      this.$nextTick(() => {
        const rollBoxHeight = this.$refs['scrollContainer' + this.id].clientHeight || this.$refs['scrollContainer' + this.id].offsetHeight;// eslint-disable-line
        this.showAmount = rollBoxHeight / window.parseInt(this.rollHeight);
        this.scroll();
      });
    },
  },
};
</script>

<style lang="less" scoped>
    .scroll_container{
      width: 100%;
      height: 100%;
      overflow: hidden;
      .anim{
        transition: all 0.5s;
      }
      ul{
        padding: 0;
        margin: 0;
        li.roll_item{
          padding: 0;
          margin: 0;
          list-style: none;
        }
      }
    }
</style>
