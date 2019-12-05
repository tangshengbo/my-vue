<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <label> 商品
      <input type="text" v-model='text'>
    </label>
    <label>
      姓名
      <input type="text" v-model='name'>
    </label>
    <label>
      年龄
      <input type="text" v-model='age'>
    </label>
    <button @click="addGood">添加</button>
    <ul>
      <li v-for='good in goods'>
        {{good.text}}
      </li>
    </ul>
    <!--    <table>-->
    <!--      <tr>-->
    <!--        <th>id</th>-->
    <!--        <th>name</th>-->
    <!--        <th>money</th>-->
    <!--        <th>createDate</th>-->
    <!--      </tr>-->

    <!--      <tr v-for="account in accountList">-->
    <!--        <td>{{account.id}}</td>-->
    <!--        <td>{{account.name}}</td>-->
    <!--        <td>{{account.money}}</td>-->
    <!--        <td>{{account.createDate}}</td>-->
    <!--      </tr>-->
    <!--      <el-button type="success">测试</el-button>-->
    <!--    </table>-->
    <div style="width: 50%">
      <el-table
        :data="accountList"
        :row-class-name="tableRowClassName"
        style="width: 100%"
        stripe
        height="500"
        border>
        <el-table-column
          prop="id"
          label="编号"
          width="180">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="180">
        </el-table-column>
        <el-table-column
          prop="money"
          label="金额"
          width="180">
        </el-table-column>
        <el-table-column
          prop="createDate"
          label="创建时间"
          width="180"
        >
        </el-table-column>

        <el-table-column
          prop="createDate"
          label="操作">
          <template slot-scope="scope">
            <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>

      </el-table>
    </div>
    <div id="app">
      <v-chart class="my-chart" :options="bar"/>
    </div>
  </div>
</template>
<style>
  .el-table .warning-row {
    background: #e3f9e2;
  }

  .el-table .success-row {
    background: #e3f9e2;
  }
</style>

<script>
  import axios from 'axios';
  import * as qs from "qs";
  import ECharts from "vue-echarts/components/ECharts";
  import "echarts/lib/chart/bar";

  axios.defaults.baseURL = 'http://localhost:8085/portal';
  export default {

    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App 唐波',
        age: '',
        name: '',
        showName: false,
        text: '',
        accountList: [],
        goods: [
          {text: 'JAVA'},
          {text: 'Web'},
          {text: 'Vue'},
          {text: '看看'}
        ],
        bar: {
          title: {
            text: "ECharts 入门示例"
          },
          tooltip: {},
          legend: {
            data: ["销量"]
          },
          xAxis: {
            data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
          },
          yAxis: {},
          series: [
            {
              name: "销量",
              type: "bar",
              data: [5, 20, 36, 10, 10, 20]
            }
          ]
        }
      };
    },
    components: {
      "v-chart": ECharts
    },
    created() {

      axios.post("/account/list", qs.stringify({
        name: this.name,
        age: this.age
      })).then(value => {
        console.info(value.data);
        console.info(value.status);
        this.accountList = value.data.data;
        this.bar.xAxis.data = this.accountList.map(account => {
          return account.name;
        });

        this.bar.series[0].data = this.accountList.map(account => {
          return account.id;
        })


      });

      setTimeout(() => {
        this.showName = true;
        console.info("要开始执行了");
        this.goods.push({text: "sss"});

      }, 10);
    },
    methods: {
      handleClick(row) {
        console.info(row);

        this.$message({
          message: row,
          type: 'success'
        });

      },

      tableRowClassName({row, rowIndex}) {
        if (rowIndex % 2 === 0) {
          return 'warning-row';
        }
        return 'success-row';
      },
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
