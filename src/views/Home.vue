<template>
  <div class="home">
    <div class="scroll">
      <scroll-box
        :rollData = data
        rollHeight ='20px'
        interval = '1000'
        rollType = 'dom'
        :roll = "roll"
      >
        <p class="item" slot-scope="props">{{props.item}}</p>
      </scroll-box>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import scrollBox from '@/components/scrollBox/index.vue';
import { setTimeout } from 'timers';

export default {
  name: 'home',
  components: {
    scrollBox,
  },
  data() {
    return {
      data: ['a', 'aa', 'aaa', 'aaaa', 'aaaaa', 'aaaaaa', 'aaaaaaa', 'aaaaaaaa', 'aaaaaaaaa', 'a', 'aa', 'aaa', 'aaaa', 'aaaaa', 'aaaaaa', 'aaaaaaa', 'aaaaaaaa', 'aaaaaaaaa'],
      roll: true,
    };
  },
  mounted() {
    setTimeout(() => {
      this.data = ['b', 'bb', 'bbb', 'bbbb', 'bbbbb', 'bbbbbb', 'bbbbbbb', 'bbbbbbbb', 'bbbbbbbbb', 'bbbbbbbbbb', 'bbbbbbbbbbb', 'bbbbbbbbbbb'];
    }, 5000);
    setTimeout(() => {
      this.roll = false;
    }, 300000);
  },
};
</script>
<style lang="less" scoped>
.home{
  .scroll{
    width: 200px;
    border: 1px solid red;
    padding: 10px;
    box-sizing: border-box;
    position: absolute;
    left: 50px;
    top: 100px;
    bottom: 0;
    .item{
      padding: 0;
      margin: 0;
      line-height: 20px;
    }
  }
}
</style>
