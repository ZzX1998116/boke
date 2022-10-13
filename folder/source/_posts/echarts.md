---
title: ECharts学习之路.....
date: 2022-10-13 15:40:00
cover: Chart,Planet
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
			xAxis: {            //直角坐标系grid中的y轴
				data: ["一", "二", "三", "四"]
			},
			yAxis: {},          //直角坐标系grid中的x轴
			series: [{
				type: "bar",
				data: ["80", "90", "70", "100"],
				name: '成绩'
			}]
		}
   {% endcodeblock %}
### 更新option
{% codeblock lang:objc %}
echart.setOption(option);
{% endcodeblock %}
## 柱状图 -- bar
## 折线图 -- line
## 平滑线 -- type:'line';smooth:true
## 面     -- aresStyle:{color:'#f00'}
## 饼形图 -- pie
