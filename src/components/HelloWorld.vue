<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p v-if="showName">{{age}}</p>
    <input type="text" v-model='text'>
    <button @click="addGood">添加</button>
    <ul>
      <li v-for='good in goods'>
        {{good.text}}
      </li>
    </ul>
  </div>
</template>

<script>

  export default {

    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App 唐波',
        age: '坦克开始看建发生的',
        showName: false,
        goods: [
          {text: 'JAVA'},
          {text: 'Web'},
          {text: 'Vue'},
          {text: '看看'}
        ]
      }
    },
    created() {
      setTimeout(() => {
        this.showName = true;
        console.info("要开始执行了");
        this.goods.push({text: "sss"});

      }, 10)
    },
    methods: {
      addGood() {
        if (this.text) {
          this.goods.push({
            text: this.text
          });
          this.text = ''
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
