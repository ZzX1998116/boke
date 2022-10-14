---
title: ECharts学习之路.....
date: 2022-10-13 15:40:00
cover: girl 
tags: [图表,ECharts]
---
ps:反正就是把所有标签的属性都整理记录一下，我记忆力差没办法记住那么多，用的时候对照着改就行了。好想摆烂啊！！！！
<!-- more -->
## option
### 参数
   {% codeblock lang:objc %}
   var option = {
			title: {            //图表的标题
				text: "vue成绩分析图"
			},
			legend: {           //展现了不同系列的标记、颜色、名字
				data: ["成绩"]
			},
			tooltip: {},        //提示框
			xAxis: {            //直角坐标系grid中的x轴
				data: ["一", "二", "三", "四"]
			},
			yAxis: {},          //直角坐标系grid中的y轴
			series: [{          //系列列表
				type: "bar",      //决定自己的图表类型
				data: ["80", "90", "70", "100"],
				name: '成绩'
			}]
		}
   {% endcodeblock %}
### 更新option
{% codeblock lang:objc %}
echart.setOption(option);
{% endcodeblock %}
### 特殊样式
{% codeblock lang:objc %}
lineStyle:{width:"10px",cap:"rouned"}
//10像素，端点平滑
itemStyle:{borderRadius:[100,100,100,100],}
//圆角，左上，右上，右下，左下
{% endcodeblock %}
## 柱状图 -- bar
### option
{% codeblock lang:objc %}
// 基于准备好的dom，初始化echarts实例
function a1() {
    var echar=echarts.init(document.getElementById("first"));
    // 指定图表的配置项和数据
    var option={
        //设置标题
        title:{
          text:"柱状图"，              //主标题
          textStyle:{
            color:"#fff"              //主标题内容样式
          },
          subtext:'柱状图哟柱状图',    //副标题
          subtextStyle:{
            color:"#fff"              //副标题内容样式
          },
          padding:[0,0,100,100]       //标题位置,因为图形是放在一个dom中,因此用padding属性来定位
          },
        //设置提示
        tooltip:{show:true},
        //设置图例
        legend:{
          type:'plain',                //类型,默认为'plain',当图例很多时可使用'scroll'
          top:'1%',                    //相对容器位置,'top\bottom\left\right'
          selected:{
            '销售':true,                //选择,图形加载出来会显示选择的图例,默认为true
          },
          textStyle:{                   //内容样式
            color:'#fff',               //所有图例的字体颜色
            backgroundColor:'black',    //所有图例的字体背景色
          },
          tooltip:{                      //图例提示框,默认不显示
            show:true,
            color:'red',
          },
          data:[                          //图例内容
            {
              name:'销售',
              icon:'circle',               //图例的外框样式
              textStyle:{
                color:'#fff',               //单独设置某一个图例的颜色
                backgroundColor:'black'     //单独设置某一个图例的字体背景色
              },
            }
          ],
          },
          //提示框
          oltip:{
            show:true,                      // 是否显示提示框,默认为true
            trigger:'item',                 //数据项图形触发
            axisPointer:{                   //指示样式
              axis:'auto'
              type:'shadow'
            },
            padding:5,
            textStyle:{                     //提示框内容
              color:'#fff'
            },
          },
        //grid区域
        grid:{
            	show:false,					//---是否显示直角坐标系网格
            	top:80,						//---相对位置，top\bottom\left\right  
            	containLabel:false,			//---grid 区域是否包含坐标轴的刻度标签
            	tooltip:{					//---鼠标焦点放在图形上，产生的提示框
            		show:true,	
            		trigger:'item',			//---触发类型
            		textStyle:{
            			color:'#666',
            		},
            	}
            },
        //设置坐标轴
        xAxis: {
            	show:true,					//---是否显示
            	position:'bottom',			//---x轴位置
            	offset:0,					//---x轴相对于默认位置的偏移
            	type:'category',			//---轴类型，默认'category'
            	name:'月份',				//---轴名称
            	nameLocation:'end',			//---轴名称相对位置
            	nameTextStyle:{				//---坐标轴名称样式
            		color:"#fff",
            		padding:[5,0,0,-5],	//---坐标轴名称相对位置
            	},
            	nameGap:15,					//---坐标轴名称与轴线之间的距离
            	//nameRotate:270,			//---坐标轴名字旋转
            	
            	axisLine:{					//---坐标轴 轴线
            		show:true,					//---是否显示
            		
            		//------------------- 箭头 -------------------------
            		symbol:['none', 'arrow'],	//---是否显示轴线箭头
            		symbolSize:[8, 8] ,			//---箭头大小
            		symbolOffset:[0,7],			//---箭头位置
            		
            		//------------------- 线 -------------------------
            		lineStyle:{
            			color:'#fff',
            			width:1,
            			type:'solid',
            		},
            	},
            	axisTick:{					//---坐标轴 刻度
            		show:true,					//---是否显示
            		inside:true,				//---是否朝内
            		lengt:3,					//---长度
            		lineStyle:{
            			//color:'red',			//---默认取轴线的颜色
            			width:1,
            			type:'solid',
            		},
            	},
            	axisLabel:{					//---坐标轴 标签
            		show:true,					//---是否显示
            		inside:false,				//---是否朝内
            		rotate:0,					//---旋转角度	
            		margin: 5,					//---刻度标签与轴线之间的距离
            		//color:'red',				//---默认取轴线的颜色
            	},
            	splitLine:{					//---grid 区域中的分隔线
            		show:false,					//---是否显示，'category'类目轴不显示，此时我的X轴为类目轴，splitLine属性是无意义的
            		lineStyle:{
            			//color:'red',
            			//width:1,
            			//type:'solid',
            		},
            	},
            	splitArea:{					//--网格区域
            		show:false,					//---是否显示，默认false
            	},           	
                data: ["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"],//内容
            },
        yAxis: {
            	show:true,					//---是否显示
            	position:'left',			//---y轴位置
            	offset:0,					//---y轴相对于默认位置的偏移
            	type:'value',			//---轴类型，默认'category'
            	name:'销量',				//---轴名称
            	nameLocation:'end',			//---轴名称相对位置value
            	nameTextStyle:{				//---坐标轴名称样式
            		color:"#fff",
            		padding:[5,0,0,5],	//---坐标轴名称相对位置
            	},
            	nameGap:15,					//---坐标轴名称与轴线之间的距离
            	//nameRotate:270,			//---坐标轴名字旋转
            	
            	axisLine:{					//---坐标轴 轴线
            		show:true,					//---是否显示
            		
            		//------------------- 箭头 -------------------------
            		symbol:['none', 'arrow'],	//---是否显示轴线箭头
            		symbolSize:[8, 8] ,			//---箭头大小
            		symbolOffset:[0,7],			//---箭头位置
            		
            		//------------------- 线 -------------------------
            		lineStyle:{
            			color:'#fff',
            			width:1,
            			type:'solid',
            		},
            	},
            	axisTick:{					//---坐标轴 刻度
            		show:true,					//---是否显示
            		inside:true,				//---是否朝内
            		lengt:3,					//---长度
            		lineStyle:{
            			//color:'red',			//---默认取轴线的颜色
            			width:1,
            			type:'solid',
            		},
            	},
            	axisLabel:{					//---坐标轴 标签
            		show:true,					//---是否显示
            		inside:false,				//---是否朝内
            		rotate:0,					//---旋转角度	
            		margin: 8,					//---刻度标签与轴线之间的距离
            		//color:'red',				//---默认取轴线的颜色
            	},
            	splitLine:{					//---grid 区域中的分隔线
            		show:true,					//---是否显示，'category'类目轴不显示，此时我的y轴为类目轴，splitLine属性是有意义的
            		lineStyle:{
            			color:'#666',
            			width:1,
            			type:'dashed',			//---类型
            		},
            	},
            	splitArea:{					//--网格区域
            		show:false,					//---是否显示，默认false
            	}                        
            },
        //设置数据
        series: [
	            {
	                name: '销量',				//---系列名称
	                type: 'bar',				//---类型
	                legendHoverLink:true,		//---是否启用图例 hover 时的联动高亮
	                label:{						//---图形上的文本标签
	                	show:false,
	                	position:'insideTop',	//---相对位置
	                	rotate:0,				//---旋转角度
	                	color:'#eee',
	                },
	                itemStyle:{					//---图形形状
	                	color:'blue',
	                	barBorderRadius:[18,18,0,0],
	                },
	                barWidth:'20',				//---柱形宽度
	                barCategoryGap:'20%',		//---柱形间距
	                data: [3020, 4800, 3600, 6050, 4320, 6200,5050,7200,4521,6700,8000,5020]
	            }
            ]
    };
    // 使用刚指定的配置项和数据显示图表。
    echar.setOption(option);
}
{% endcodeblock %}
### 分析
{% image ./2018041500134284.png download:./2018041500134284.png fancybox:true %}
## 折线图 -- line
{% codeblock lang:objc %}
option = {
    //设置线条的颜色，后面是个数组
    color:['pink','red','green','blue','gray'],
    //设置图表标题
    title: {
        text: '折线图堆叠1233标题'
    },
    //图表的提示框组件
    tooltip: {
        //触发方式 - 坐标轴
        trigger: 'axis'
    },
    //图例组件
    legend: {
        //series有name了，这里的data可以删除掉
        data: ['邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎']
    },
    //网格配置 grid可以控制线形图 柱状图 图标大小
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        //是否显示刻度标签
        containLabel: true
    },
    //工具箱组件，可以另存为图片等功能
    toolbox: {
        feature: {
            saveAsImage: {}
        }
    },
    //设置x轴的相关配置
    xAxis: {
        type: 'category',
        //线条和y轴是否有缝隙
        boundaryGap: false,
        data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
    },
    //设置y轴的相关配置
    yAxis: {
        type: 'value'
    },
    //系列图表配置，决定显示那种类型的图表
    series: [
        {
            name: '邮件营销',
            type: 'line',
            //总量，后面的会堆叠前面的累加起来，删除掉就会折叠了，一般不需要
            //stack: '总量',
            data: [120, 132, 101, 134, 90, 230, 210]
        },
        {
            name: '联盟广告',
            type: 'line',
            //stack: '总量',
            data: [220, 182, 191, 234, 290, 330, 310]
        },
        {
            name: '视频广告',
            type: 'line',
            //stack: '总量',
            data: [150, 232, 201, 154, 190, 330, 410]
        },
        {
            name: '直接访问',
            type: 'line',
            //stack: '总量',
            data: [320, 332, 301, 334, 390, 330, 320]
        },
        {
            name: '搜索引擎',
            type: 'line',
            //stack: '总量',
            data: [520, 932, 901, 934, 1290, 1330, 1320]
        }
    ]
};

{% endcodeblock %}
## 平滑线 -- type:'line';smooth:true
## 面     -- aresStyle:{color:'#f00'}
## 饼形图 -- pie
