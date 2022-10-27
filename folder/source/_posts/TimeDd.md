---
title: 仿造叮咚买菜的时间选择器（伪）
date: 2022-10-18 17:10:00
cover: ./uncategorized/TimeDd/1.jpg
cover_info:
  meta: 仿造叮咚买菜的时间选择器（伪）
  title: 遇到这个在网上浅搜了一下没搜到，想着自己写一个。可能很简单，但是...我喜欢
tags: [时间选择器,uniapp,插件,uview]
---

<!-- more -->

##时间选择器
### 配置
{% codeblock lang:objc %}
calculation(star,interval) 
// 第一个参数是开始的时间，也就是可选的时间的区间开始的时间
// 第二个参数是区间的扩展性，0.5的话是半小时；1的话是一小时，以此类推 (还没做到)
timestamp(star,date)
// 第一个参数是开始的时间戳，
{% endcodeblock %}

### 代码
父组件
{% codeblock lang:objc %}
<button @click="gotoPopup">点击</button>
methods: {
			gotoPopup(){
				this.$refs.insled.show = true
			}
		}
{% endcodeblock %}
子组件
{% codeblock lang:objc %}

<template>
	<u-popup :show="show" mode="bottom" :round="10" @close="close" closeable>
		<div class="Title">选择送达时间</div>
		<div class="Time">
			<div class="Time_YMD">
				<u-list @scrolltolower="scrolltolowerYMD" height="100%">
					<u-list-item>
						<p class="YMD_p">YMDtime</p>
					</u-list-item>
				</u-list>
			</div>
			<div class="Time_HMS">
				<u-list @scrolltolower="scrolltolowerHMS" height="100%">
					<u-list-item v-for="(val ,ind) in timeArray">
						<div class="HMS_li" @click="onHMSLi(val)">val</div>
					</u-list-item>
				</u-list>
			</div>
		</div>
	</u-popup>
</template>

<script>
	export default {
		data() {
			return {
				show: false,
				YMDtime: "",
				timeArray: [],
				HMSindex:null,
			}
		},
		methods: {
			scrolltolowerYMD() {},
			scrolltolowerHMS() {},
			calculation(star, interval) {
				let Home = star.split(':')[0]
				let front = Number(Home)
				let after = Number(front) + 1
				for (let i = Home; i < 24*Number(interval); i++) {
					this.timeArray.push(front + ':00 - ' + after + ':00')
					front = front + 1
					after = front + 1
				}
			},
			timestamp(star, date) {
				let newHMS = Number(star) + (24 * 60 * 60 * 1000) * Number(date)
				let time = new Date(newHMS)
				let Y = time.getFullYear() + '年'
				let M = (time.getMonth() + 1 < 10 ? '0' + (time.getMonth() + 1) : time.getMonth() + 1) + '月'
				let D = (time.getDate() < 10 ? '0' + time.getDate() : time.getDate()) + '日'
				return Y + M + D
			},
			onHMSLi(val){
				this.show = false
			},
			close(){
				this.show = false
			}
		},
		mounted() {
			var myDate = new Date();
			myDate.getTime();
			this.YMDtime = this.timestamp(myDate.getTime(), 1)
			var mytime = myDate.toLocaleTimeString(); //获取当前时间
			this.calculation("00:00:00", 1)
		}
	}
</script>

<style lang="scss" scoped>
	.Title {
		text-align: center;
		padding: 40rpx;
		border-bottom: 1px solid #f4f4f4;
	}

	.Time {
		display: flex;
		justify-content: space-between;
		background-color: #e7e7e7;
		.Time_YMD,
		.Time_HMS {
			height: 50vh;
		}
		.Time_YMD{width: 40vw;}
		.Time_HMS{width: 60vw;}
		.YMD_p{
			font-size: 14px;
			background-color: #fff;
			padding: 15px 10px;
			text-align: center;
			color: #5AAF76;
			font-weight: 600;
		}
		.HMS_li{
			font-size: 14px;
			border-bottom: 1px solid #eaeaea;
			background-color: #fff;
			padding:15px 10px;
		}
	}
</style>


{% endcodeblock %}