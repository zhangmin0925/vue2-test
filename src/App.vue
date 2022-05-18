<template>
  <div id="app">
    <!-- 1.实现点击，让元素动画平移 -->
    <img :style="{ transform: `translateX(${xVal})`}" class="img" ref="imgref" @click="handleClick" alt="Vue logo" src="./assets/logo.png">
    <!-- 2.实现loading( 1/4 个圆弧旋转 ) -->
    <div class="curcle"></div>
    <!-- 长列表优化 TODO https://www.vue-js.com/topic/6111fd46bef6720032ceb029-->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <!-- computed运行逻辑 -->
    <div>nickName{{nickName}}</div>
    <div>firstName{{firstName}}</div>
    <div>lastName{{lastName}}</div>
    <div>name{{name}}</div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },
  data() {
    return {
      count: 1,
      xVal: '0px',
      nickName: "",
      firstName: "",
      lastName: ""
    }
  },
  computed: {
      name() {
        console.log(666,'computed执行了')
        // 也会添加watcher
        if(this.nickName) {
          console.log(111)
        }
        return this.firstName + this.lastName;
      }
  },
  mounted() {
    setTimeout(() => {
      // 这些操作分步骤执行，computed都会执行
      // 这些步骤合并执行，computed只执行一次
      this.firstName = 'test'
      this.lastName = 'test'
      this.nickName = 'test'
    }, 3000)
  },
  methods: {
    handleClick() {
      console.log(this.$refs.imgref.style.transform)
      this.$refs.imgref.style.transform = `translateX(${this.count * 100}px)`
      this.$refs.imgref.style.backgroundColor = 'red'
      this.count++
      this.mockLoadImg()
    },
    // 3.实现图片懒加载
    mockLoadImg() {
      // 最好包一层promise, resolve(img)出去
      let img = new Image()
      img.src = 'http://localhost:8080/img/logo.82b9c7a5.png'
      img.alt = 'vue logo'
      img.onload = function() {
        console.log('success')
      }
      document.getElementById('app').appendChild(img)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  position: relative;
}
.img {
  /* 状态值发生改变就会触发transition动画
  /* animation与transition 不同的是，keyframes提供更多的控制，这点让css animation更加强大 */ 
  transition: ease 4000ms ;
}
.curcle {
  width: 0; 
  height: 0; 
  border: 50px solid; 
  /* 四边颜色：一边有色就行（上右下左），其余透明 */
  border-color: transparent transparent red transparent; 
  /* 边圆角 */
  border-radius: 50%;
  /* 旋转动画 */
  animation: ani 3s infinite linear;
}
@keyframes ani {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
