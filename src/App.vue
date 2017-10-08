<template>
  <div id="app" class="todolistClass">
    <h1 v-text="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNewItem"></input>
    <button @click="addNewItem" class="button blue">Add</button>
    <lable v-text="errMsg" v-if="errMsgVisiable" v-bind:class="{ errMsgStyle: this.errMsgVisiable }"></lable>
    <ul>
      <li v-for="(item, index1) in items" :key="item.id">
        <div class="liDiv">
          <input type="checkbox" v-model="item.isfinished">
          <label v-on:click="finishThis(item)" v-bind:class="{ finished: item.isfinished}"> {{item.lable}} </label>
          <button v-on:click="removeThis(item,index1)" class="xBtn">X</button>
        </div>
      </li>
    </ul>
    <!-- 注意若不使用v-bind,直接使用haveDoneNo='1','1'不能用双引号，否则会导致haveDoneNoUndefine -->
    <done v-bind:haveDoneNo="getListNo(items)" v-on:chlidTalk="printChlidWord"></done>
    <!-- <p> childsay: {{ chlidWord }} </p> 显示子组件文本-->
    <img :src="chlidWord">
    <!-- <done v-bind="items"></done> 为什么这样不行，items传到子组件显示undefined-->
  </div>
</template>

<script>
import Store from './store'
import Done from './components/Done'

export default {
  data () {
    return {
      title: 'This is a todo list',
      items: Store.fetch(),
      errMsg: '',
      errMsgVisiable: false,
      chlidWord: '',
      // imgVisiable: false,
      // listNo: this.getListNo(),
      // items: [
      //   {
      //     lable: '看书',
      //     isfinished: false
      //   },
      //   {
      //     lable: '敲代码',
      //     isfinished: true
      //   }
      // ],
      // 赋初始值
      newItem: ''
    }
  },
  methods: {
    finishThis: function (item) {
      item.isfinished = !item.isfinished
      // console.log(item)
    },
    addNewItem: function () {
      if (this.newItem === '') {
        this.errMsg = '禁止为空'
        this.errMsgVisiable = true
        this.newItem = ''
        return ''
      }
      this.errMsg = ''
      this.errMsgVisiable = false
      this.items.push({
        lable: this.newItem,
        isfinished: false
      })
      this.newItem = ''
    },
    removeThis: function (item, index) {
      // console.log(item, index)  /* object 索引值 */
      // console.log(JSON.stringify(item)) /* {"lable":"2","isfinished":false} */
      // console.log(JSON.stringify(this.items)) /* [{"lable":"1","isfinished":false},{"lable":"2","isfinished":false},{"lable":"3","isfinished":false}] */
      // console.log(this.items) /* 列表对象 (2) [{…}, {…}, __ob__: Observer] */
      // console.log(this.items.indexOf[item])  /* undefined为什么???? */
      this.items.splice(index, 1)  /* 1表示删几行 */
    },
    // 为什么不认this.items?
    getListNo: function (items) {
      // console.log(items)
      var finNo = 0
      var talNo = 0
      // ???此处的obj为什么是string型的index值
      for (var obj in items) {
        // console.log(obj)
        if (items[obj].isfinished) {
          finNo++
        }
        talNo++
      }
      // console.log(i)
      return {
        finishNo: finNo,
        totalNo: talNo
      }
    },
    printChlidWord: function (msg) {
      var listNo = this.getListNo(this.items)
      console.log(listNo)
      // console.log(this.getListNo(this.items).finishNo)
      if (listNo.finishNo === listNo.totalNo) this.chlidWord = msg
    }
  },
  watch: {
    items: {
      handler: function (items) {
        // console.log(val,oldVal)
        Store.save(items)
      },
      // 深拷贝：否则不会认为是同一个对象
      deep: true
    }
  },
  components: { Done }
}
</script>

<style>
h1 {
  color: #00678e
}
.finished {
  text-decoration: underline
}

.errMsgStyle {
  color: red;
  font-size: 1px
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todolistClass{
  margin:200px auto;
  width:400px;
  height: auto;
}
.liDiv {
  margin-top: 10px;
}
.xBtn{		
  opacity: 0.5;
  border: none;
  margin-left: 10px;
}
.button {
	display: inline-block;
	outline: none;
	cursor: pointer;
	text-align: center;
	text-decoration: none;
	font: 14px/100% Arial, Helvetica, sans-serif;
	padding: .5em 2em .55em;
	text-shadow: 0 1px 1px rgba(0,0,0,.3);
	-webkit-border-radius: .5em; 
	-moz-border-radius: .5em;
	border-radius: .5em;
	-webkit-box-shadow: 0 1px 2px rgba(0,0,0,.2);
	-moz-box-shadow: 0 1px 2px rgba(0,0,0,.2);
	box-shadow: 0 1px 2px rgba(0,0,0,.2);
}
.button:hover {
	text-decoration: none;
}
.button:active {
	position: relative;
	top: 1px;
}

.blue {
	color: #d9eef7;
	border: solid 1px #0076c3;
	background: #0095cd;
	background: -webkit-gradient(linear, left top, left bottom, from(#faa51a), to(#f47a20));
	background: -moz-linear-gradient(top,  #00cdee,  #0078a5);
	filter:  progid:DXImageTransform.Microsoft.gradient(startColorstr='#faa51a', endColorstr='#f47a20');
}
.blue:hover {
	background: #007ead;
	background: -webkit-gradient(linear, left top, left bottom, from(#f88e11), to(#f06015));
	background: -moz-linear-gradient(top,  #0095cc,  #00678e);
	filter:  progid:DXImageTransform.Microsoft.gradient(startColorstr='#f88e11', endColorstr='#f06015');
}
.blue:active {
	color: #88bed6;
	background: -webkit-gradient(linear, left top, left bottom, from(#f47a20), to(#faa51a));
	background: -moz-linear-gradient(top,  #0078a5,  #00cdee);
	filter:  progid:DXImageTransform.Microsoft.gradient(startColorstr='#f47a20', endColorstr='#faa51a');
}
</style>