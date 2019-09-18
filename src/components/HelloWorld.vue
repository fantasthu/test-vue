<template>
  <div class="hello">
    <!-- 父子组件声明周期的体验 -->
    <el-card class="card">
      <div class="title">父子组件声明周期的体验(打开控制台)</div>
      <el-button @click="handleClick">我来改变数据</el-button>
      <el-button @click="handleChangeChild">我来改变我儿子</el-button>
      <el-button @click="handleDestroyChild">我来灭了我儿子</el-button>
      <!-- <Child :message="childMessage" :childData="childData" v-if="childShow" /> -->
      <ul>
        <li v-for="item in list" :key="item.index">{{ item.value }}</li>
      </ul>
    </el-card>
    <!-- v-model的应用 -->
    <el-card class="card">
      <div class="title">v-model语法糖的应用</div>
      <el-button @click="showModel">占时弹框</el-button>
      <ModelChild v-model="showModelStatus" />
      {{ showModelStatus }}
    </el-card>
    <!-- this.$nextTick的应用 -->
    <el-card class="card">
      <div class="title" @click="toggleClick">this.$nextTick的应用</div>
      <div class="tick-des">
        <textarea v-show="showInput" class="tick-content" :value="input" @input="update"></textarea>
        <div v-html="compiledMarkdown" style="text-align:left"></div>
      </div>
    </el-card>
    <!-- slot的应该 -->
    <el-card class="card">
      <div class="title">slot的应用</div>
      <div class="content">
        <div class="h3">子组件的定义:</div>
        <div class="h3-des" v-text="slothtml1" style="white-space: pre-wrap;"></div>
        <div class="h3">父组件中的使用:</div>
        <div class="h3-des" v-text="slothtml2" style="white-space: pre-wrap;"></div>
      </div>
    </el-card>
    <!-- 动态组件 -->
    <el-card class="card">
      <div class="title">动态组件的使用</div>
      <div class="content">
        <div class="h3">应用</div>
        <div class="h3-des" v-text="slothtml3" style="white-space: pre-wrap;"></div>
      </div>
    </el-card>
    <!-- mixins 混入 -->
    <el-card class="card">
      <div class="title">mixins混用</div>
      <div class="content">
        <div class="h3">mixin类似父子类的概念,先执行生命周期,子类可以使用父类方法</div>
        <div class="h3-des" style="white-space: pre-wrap;">
          参考地址:
          https://juejin.im/entry/586b06811b69e60063f29002
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
import Child from "./Child";
import ModelChild from "./ModelChild";
import list from "../mixins/list";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  mixins: [list],
  components: { Child, ModelChild },
  data() {
    return {
      list: null,
      childData: null,
      childMessage: "",
      childShow: true,
      showModelStatus: false,
      input: "",
      showInput: false,
      slothtml1: "",
      slothtml2: "",
      slothtml3: ""
    };
  },
  computed: {
    compiledMarkdown: function() {
      return marked(this.input, { sanitize: true });
    }
  },
  methods: {
    toggleClick() {
      this.showInput = !this.showInput;
    },
    update: _.debounce(function(e) {
      this.input = e.target.value;
    }, 300),
    showModel() {
      this.showModelStatus = true;
    },
    async loadData() {
      this.list = await this.getData();
      this.childData = this.list;
      setTimeout(this.changeChildMessage, 2000);
    },
    changeChildMessage() {
      this.childMessage = "我是你2000毫秒的儿子";
    },
    handleChangeChild() {
      this.childMessage = "我改变了儿子";
    },
    handleDestroyChild() {
      this.childShow = false;
    },
    getData() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          const arr = [];
          for (let index = 0; index < 1000; index++) {
            arr.push({
              index: index,
              value: `${index}}-hwz`
            });
          }
          resolve(arr);
        }, 1000);
      });
    },
    handleClick() {
      this.list.unshift({
        index: 1000001,
        value: "我来搞事"
      });
    },
    marked() {
      let str = "\n\r### 官网文档说明:";
      str +=
        "\n ```注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted``` ";
      str += "\n\r mounted: function () {";
      str += "\n\r &nbsp;&nbsp;this.$nextTick(function () {";
      str += "\n\r &nbsp;&nbsp;})";
      str += "\n\r} ";
      str += "\n\r ### 示例展示: ";
      str += "\n\r 前提是keywords是隐藏着的 ";
      str += "\n\r showsou(){ ";
      str += "\n\r  &nbsp;&nbsp;this.showit = true ";
      str += "\n\r  &nbsp;&nbsp;this.$nextTick(function () {";
      str += "\n\r  &nbsp;&nbsp;document.getElementById('keywords').focus() ";
      str += "\n\r} })";
      this.input = this.input + str;
    },
    slotHtml() {
      this.slothtml1 += '<div class="box">\n\r';
      this.slothtml1 += '  <div class="title"></div>\n\r';
      this.slothtml1 += '  <slot name="myslot"></slot>\n\r';
      this.slothtml1 += "</div>";
      this.slothtml2 += "<Child>\n\r";
      this.slothtml2 += '  <div slot="myslot" scope="props"></div>\n\r';
      this.slothtml2 += "</Child>\n\r";
      this.slothtml2 += '<component :is="currentView"></component> \n\r';
      this.slothtml3 += "vue实例代码如下: \n\r";
      this.slothtml3 += "var vm = new Vue({ \n\r";
      this.slothtml3 += 'el: "#example", \n\r';
      this.slothtml3 += " data: { \n\r";
      this.slothtml3 += "currentView: 'home' \n\r";
      this.slothtml3 += " },\n\r";
      this.slothtml3 += "components: { home,posts,archive }\n\r";
      this.slothtml3 += "}) \n\r";
      this.slothtml3 += " \n\r";
      this.slothtml3 +=
        "如果你想将切换渲染过的组件保留在内存中可以使用keep-alive将动态组件包裹:\n\r";
      this.slothtml3 += "<keep-alive>\n\r";
      this.slothtml3 += '<component :is="currentView">\n\r';
      this.slothtml3 += "...\n\r";
      this.slothtml3 += "</component>\n\r";
      this.slothtml3 += "<keep-alive>\n\r";
      this.slothtml3 += "\n\r";
    }
  },

  beforeCreate() {
    console.log("beforeCreated :", "beforeCreated");
  },
  created() {
    this.loadData();
    console.log("created :", "created");
  },
  mounted() {
    console.log("mounted :", "mounted");
    this.marked();
    this.slotHtml();
  },
  beforeUpdate() {
    console.log("beforeUpdate :", "beforeUpdate");
  },
  updated() {
    console.log("updated :", "updated");
  },
  watch: {
    list: {
      handler(val) {
        console.log("val :", val);
      },
      deep: true
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
.title {
  font-weight: bold;
  margin-bottom: 30px;
}
.card {
  width: 500px;
  height: 500px;
  overflow: scroll;
  float: left;
}
.clearfix {
  clear: both;
}
.tick-content {
  display: inline-block;
  width: 80%;
  height: 300px;
}
.h3 {
  font-weight: bold;
  text-align: left;
  margin-bottom: 15px;
}
.h3-des {
  text-align: left;
  background-color: #eee;
  padding: 15px;
}
</style>
