<template>
  <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
</template>



<script>
  export default {
    data() {
      return {
        tmpArr: [{ label: '2', children: [] }],
        data: [],
        defaultProps: {
          children: 'children',
          label: 'label'
        }
      };
    },
    mounted: function () {
      this.$http.jsonp('https://api.douban.com/v2/movie/top250?count=10', {}, {
        headers: {

        },
        emulateJSON: true
      }).then(function (response) {
        var testData=[];
        testData = response.data.subjects
        testData.forEach(function (item, index, arry) {
          item.casts.forEach(function (item, index, arry) {
            item.label = item.name;
            item.children = [];
          })
          item.label = item.title
          item.children = item.casts
        })
        this.tmpArr[0].label=response.data.title;
        this.tmpArr[0].children=testData;
        this.data = this.tmpArr
      }, function (response) {
        // 这里是处理错误的回调
        console.log(response)
      });
    },
    methods: {
      handleNodeClick(data) {
        console.log(data);
      }
    }
  };
</script>