<template>
  <div class="done">
    <h2>{{ msg + this.haveDoneNo.finishNo + '/' + this.haveDoneNo.totalNo }}</h2>
    <button @click="tellFather">AllDone点我有惊喜</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: '已完成',
      imgurl: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1507443474005&di=dae9b01057cc7a734429a8e18377dd97&imgtype=0&src=http%3A%2F%2Fe.hiphotos.baidu.com%2Fimage%2Fpic%2Fitem%2F7c1ed21b0ef41bd5414c3d3458da81cb38db3dd7.jpg'
    }
  },
  props: ['haveDoneNo'],
  methods: {
    tellFather: function () {
    //   this.$emit('chlidTalk', this.msg)
      this.$emit('chlidTalk', this.imgurl)
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
