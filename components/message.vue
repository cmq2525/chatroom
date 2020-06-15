<template>
	<div class="message" v-scroll-bottom="currentSession.messages">
		<el-button @click="refresh_message">手动刷新</el-button>
		<ul v-if="currentSession">
			<li v-for="item in currentSession.messages">
				<p class="time">
					<!-- <span>{{ item.date | time }}</span> -->
					<span>{{item.date | dateToTime}}</span>
				</p>
				<div class="main" :class="{ self: item.self }">
					<header>
						<img class="avatar" width="30" height="30" :src="get_avatar(item.name)" />
						<p class="name">{{item.name}}</p>
					</header>
					<div class="text">{{ item.content }}</div>
				</div>
			</li>
		</ul>
	</div>
</template>

<script>
	import {mapGetters} from 'vuex'
	export default {
		data() {
			return {
				timer: ''
			}
		},
		computed: {
			...mapGetters({
				currentSession: 'currentSession',
				currentId: 'currentId',
				user: 'user',
				avatars: 'avatars'
			}),

		},
		methods: {
			get_avatar(name) {
				let img = this.$axios.defaults.baseURL + this.avatars[name];
				if (!img) {
					let _this = this;
					this.$axios.get('api/get_avatar', {
						params:{
							'name':name
						}
					}).then(function(response) {
						img = response.data.img;
						_this.$store.commit('REFRESH_AVATAR', name, img);
						_this.$forceUpdate();
					});
				}
				return img;
			},
			refresh_message() {
				let _this = this;
				window.console.log(this.currentSession.now);
				let room_id = this.currentId;
				let start_time = this.currentSession.now;
				this.$axios.get('api/refresh_chat', {
					params: {
						'target_id': room_id,
						'target_type': 'chatroom',
						'start_time': start_time,
					}
				}).then(function(response) {
					window.console.log(response)
					let chat_content = response.data.chat_content;
					let end_time = response.data.end_time;
					for (let i=0;i<chat_content.length;i++) {
						let name = chat_content[i][0];
						let content = chat_content[i][1];
						let date = chat_content[i][2];
						window.console.log('name,content,date:',name,content,date)
		
						_this.$store.commit('REFRESH_SESSION', {name, content, date});
					}
					_this.$store.commit('REFRESH_TIME', end_time);
					_this.$forceUpdate();
				})
			}

		},
		filters: {
			// 将日期过滤为 hour:minutes

			dateToTime(date) {
				if (typeof date === 'string') {
					//date = new Date(date);
					return date
				}

				return date.getUTCFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate() + ' ' + date.getHours() + ':' +
					date.getMinutes();
			}
		},
		directives: {
			// 发送消息后滚动到底部
			'scroll-bottom': function(el, binding, vnode) {
				el.scrollTop = el.scrollHeight - el.clientHeight;
			}
		},
		mounted() {
			//this.timer = setInterval(this.refresh_message, 5000);
		},
		beforeDestroy() {
			clearTimeout(this.timer);
		}
	};
</script>



<style lang="less" scoped>
	.message {
		padding: 10px 15px;
		overflow-y: scroll;

		li {
			margin-bottom: 15px;
			//去掉小圆点
			list-style: none;
		}

		.time {
			margin: 7px 0;
			text-align: center;

			>span {
				display: inline-block;
				padding: 0 18px;
				font-size: 12px;
				color: #fff;
				border-radius: 2px;
				background-color: #dcdcdc;
			}
		}

		.avatar {
			float: left;
			margin: 0 10px 0 0;
			border-radius: 3px;
		}

		.name {
			font-size: 14px;
			font-family: KaiTi;
		}

		.text {
			display: inline-block;
			position: relative;
			padding: 0 10px;
			max-width: ~'calc(100% - 40px)';
			min-height: 30px;
			line-height: 2.5;
			font-size: 12px;
			text-align: left;
			word-break: break-all;
			background-color: #fafafa;
			border-radius: 4px;

			&:before {
				content: " ";
				position: absolute;
				top: 9px;
				right: 100%;
				border: 6px solid transparent;
				border-right-color: #fafafa;
			}
		}

		.self {
			text-align: right;

			.name {
				display: none;
			}

			.avatar {
				float: right;
				margin: 0 0 0 10px;
			}

			.text {
				background-color: #b2e281;

				&:before {
					right: inherit;
					left: 100%;
					border-right-color: transparent;
					border-left-color: #b2e281;
				}
			}
		}
	}
</style>
