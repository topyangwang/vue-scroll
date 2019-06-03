# vue_demo

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

# doc
## 默认为data模式
### data 模式通过改变循环data数据实现循环播放，必须传入循环数据rollData;

## rollType = 'data' 设置为data模式
## dom模式通过操作dom实现 

## interval 设置滚动间隔 单位毫秒

## rollHeight 设置一次滚动高度，一般与滚动条目高度一致

## roll 是否滚动，传入数据为false时停止滚动

## indexKey 循环中的:key的值在循环数据中的属性。不传以index作为key属性的值（不推荐）

## 完整实例
  ```html
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
  ``
  ```javascript
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
      }, 10000);
    },
  };
  </script>
  ```
  ```less
  <style lang="less" scoped>
  ul,p{
    padding: 0;
    margin: 0;
    li{
      padding: 0;
      margin: 0;
      list-style: none;
      height: 20px;
    }
  }
  .content{
    width: 200px;
    height: 300px;
    border: 1px solid red;
    margin: 0 auto;
    .scroll{
      padding: 10px;
      box-sizing: border-box;
      height: 100%;
    }
    .item{
      background: greenyellow;
      margin-bottom: 10px;
      border: 1px solid #000;
    }
  }
  </style>
  ```