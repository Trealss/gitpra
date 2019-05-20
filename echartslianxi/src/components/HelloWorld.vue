<template>
  <div class="hello">
    <div id="myChart1" class="echart"></div>
    <div id="myChart2" class="echart"></div>
    <button @click="showMychart3">show</button>
    <button @click="showMychart3_1">show1</button>
    <div id="myChart3" class="echart"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      data: ['周一','周二','周三','周四','周五','周六','周日'],
      data1: ['周日', '周一','周二','周三','周四','周五','周六'],
      classify: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"],
      claData: [5, 20, 36, 10, 10, 20],
      pieData:[
        {value:235, name:'视频广告'},
        {value:274, name:'联盟广告'},
        {value:310, name:'邮件营销'},
        {value:335, name:'直接访问'},
        {value:400, name:'搜索引擎'}
      ],
      option: {
        title : {
          text: '气温及湿度变化',
          // subtext: '温度 ℃'
        },
        tooltip : {
          trigger: 'axis'
        },
        legend: {
          data:['气温', '湿度']
        },
        dataZoom : {
          show : true,
          realtime : true,
          start : 0,
          end : 100
        },
        calculable : true,
        xAxis : [
          {
            type : 'category',
            boundaryGap : false
          }
        ],
        yAxis : [
          // y 轴数据，value里面的对应的是y轴上的均匀的数据分配
          // 自动计算好的
          {
            name : '气温(℃)',
            type : 'value',
            max: 37,
            min: 10
          },
          {
            name : '湿度(%)',
            type : 'value',
            max: 100,
            min: 20,
            x: 'right',
            show: true
          },
          {
            axisLabel : {
              show:true,
              interval: 'auto',    // {number}
              rotate: -45,
              margin: 18,
              formatter: '{value} ℃',    // Template formatter!
              textStyle: {
                color: '#1e90ff',
                fontFamily: 'verdana',
                fontSize: 10,
                fontStyle: 'normal',
                fontWeight: 'bold'
              }
            },
          },
          {
            type : 'value',
            splitNumber: 10,
            axisLabel : {
              formatter: function (value) {
                // Function formatter
                return value + ' %'
              }
            },
            splitLine : {
              show: false
            }
          }
        ],
        series : [
          {
            // 上面legend 拿的就是它和另一个哥们儿的 name 值
            // 折线图里面的描述最高最低拿的也是legend中的值
            name:'气温',
            type:'line',
            // 折线图显示的每个星期（x轴）对应的y轴的位置，形成折线图上的点
            data:[23, 27, 16, 30, 29, 26, 21],
            markPoint : {
              data : [
                // xAxis 这条数据出现在x， y轴的索引位置（最低点的描述）
                {type : 'max', name: '最大值'},
                {type : 'min', name: '最小值'}
              ]
            },
          },

          {
            name:'湿度',
            type:'line',
            yAxisIndex:1,
            data: (function(){
              //
              var oriData = [
                50, 67, 43, 88, 53, 74, 59
              ];
              var len = oriData.length;
              while(len--) {
                oriData[len] *= 1;
              }
              return oriData;
            })(),
            markPoint : {
              data : [
                // xAxis 这条数据出现在x， y轴的索引位置（最低点的描述）
                {type : 'max', name: '最大值'},
                {type : 'min', name: '最小值'}
              ]
            },
          }
        ]
      }
    }
  },
  methods: {
    showMychart3_1 () {
      this.option.xAxis[0].data = this.data1;
      this.drawLine3();
    },

    showMychart3 () {
      this.option.xAxis[0].data = this.data;
      console.log(this.option.xAxis);
      this.drawLine3();
    },

    drawLine1() {
      // 基于准备好的dom，初始化echarts实例
      let myChart1 = this.$echarts.init(document.getElementById('myChart1'))
      // 绘制图表
      myChart1.setOption({
        title: { text: '在Vue中使用echarts' },
        tooltip: {},
        xAxis: {
          data: this.classify
        },
        yAxis: {},
        series: [{
          name: '各个商品的销量',
          type: 'bar',
          data: this.claData
        }]
      });
    },
    drawLine3 () {

      let myChart3 = this.$echarts.init(document.getElementById('myChart3'));
      myChart3.setOption(this.option);
    },
    drawLine2() {
      let myChart2 = this.$echarts.init(document.getElementById('myChart2'));
      myChart2.setOption({
        series: [
          {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            data: this.pieData
          }
        ]
      })
    }
  },
  mounted() {
    this.drawLine1();
    this.drawLine2();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .echart {
    width: 800px;
    height: 600px;
    margin: 100px auto;
  }
</style>
