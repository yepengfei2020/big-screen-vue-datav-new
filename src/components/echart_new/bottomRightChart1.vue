<template>
  <div>
    <div id="bottomRightChart" style="width:11.125rem; height: 5.88rem;"></div>
  </div>
</template>

<script>
import echartMixins from "@/utils/resizeMixins";

export default {
  data() {
    return {
      chart: null,
      datas: []
    };
  },
  mixins: [echartMixins],
  mounted() {
    this.draw();
    this.charData();
  },
  methods: {
    charData() {
      this.datas =  [99,80,65,32,30,28,28]
      setInterval(() => {
        for(let i=0;i<this.datas.length;i++){
          this.datas[i] = this.RandomNum(1,500)
        }
        // console.log(this.datas)
        this.draw()
      }, 3000);
    },
    RandomNum(Min, Max) {
      var Range = Max - Min;
      var Rand = Math.random();
      if(Math.round(Rand * Range)==0){      
        return Min + 1;
      }
      var num = Min + Math.round(Rand * Range);
      return num;
    },
    draw() {
      // 基于准备好的dom，初始化echarts实例
      this.chart = this.$echarts.init(document.getElementById("bottomRightChart"));
      //  ----------------------------------------------------------------
      let  option = {
	    title:{
            text:'按类型统计',
            top:'bottom',
            left:'center',
            textStyle:{
                fontSize: 14,
                fontWeight: '',
                color: '#333'
            },
        },//标题
        tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
            /*formatter:function(val){   //让series 中的文字进行换行
                 console.log(val);//查看val属性，可根据里边属性自定义内容
                 var content = var['name'];
                 return content;//返回可以含有html中标签
             },*/ //自定义鼠标悬浮交互信息提示，鼠标放在饼状图上时触发事件
        },//提示框，鼠标悬浮交互时的信息提示
        legend: {
            show: false,
            orient: 'vertical',
            x: 'left',
            data: ['一月', '二月', '三月']
        },//图例属性，以饼状图为例，用来说明饼状图每个扇区，data与下边series中data相匹配
        graphic:{
            type:'text',
            left:'center',
            top:'center',
            style:{
                text:'用户统计\n'+'100', //使用“+”可以使每行文字居中
                textAlign:'center',
                font:'italic bolder 16px cursive',
                fill:'#000',
                width:30,
                height:30
            }
        },//此例饼状图为圆环中心文字显示属性，这是一个原生图形元素组件，功能很多
        series: [
            {
                name:'用户统计',//tooltip提示框中显示内容
                type: 'pie',//图形类型，如饼状图，柱状图等
                radius: ['35%', '65%'],//饼图的半径，数组的第一项是内半径，第二项是外半径。支持百分比，本例设置成环形图。具体可以看文档或改变其值试一试
                //roseType:'area',是否显示成南丁格尔图，默认false
                itemStyle: {
                    normal:{
                        label:{
                            show:true,
                            textStyle:{color:'#3c4858',fontSize:"18"},
                            formatter:function(val){   //让series 中的文字进行换行
                                return val.name.split("-").join("\n");}
                        },//饼图图形上的文本标签，可用于说明图形的一些数据信息，比如值，名称等。可以与itemStyle属性同级，具体看文档
                        labelLine:{
                            show:true,
                            lineStyle:{color:'#3c4858'}
                        }//线条颜色
                    },//基本样式
                    emphasis: {
                        shadowBlur: 10,
                        shadowOffsetX: 0,
                        shadowColor: 'rgba(0, 0, 0, 0.5)',//鼠标放在区域边框颜色
                        textColor:'#000'
                    }//鼠标放在各个区域的样式
                },
                data: [
                    {value: 50, name: '一月'},
                    {value: 25, name: '二月'},
                    {value: 25, name: '三月'},
                ],//数据，数据中其他属性，查阅文档
                color: ['#51CEC6','#FFB703','#5FA0FA'],//各个区域颜色
            },//数组中一个{}元素，一个图，以此可以做出环形图
        ],//系列列表
    };
      this.chart.setOption(option);
    }
  },
  destroyed() {
    window.onresize = null;
  }
};
</script>

<style lang="scss" scoped>
</style>