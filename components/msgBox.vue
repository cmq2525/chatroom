<template>
	<div class="text">
		<textarea placeholder="按 Ctrl + Enter 发送" v-model="content" @keyup="onKeyup"></textarea>
	</div>
</template>

<script>
	import {
		mapGetters
	} from 'vuex'
	export default {
		computed: {
			...mapGetters({
				currentId: 'currentId',
				user: 'user',
			}),
		},
		data() {
			return {
				content: '',

			};
		},
		methods: {
			onKeyup(e) {
				if (e.ctrlKey && e.keyCode === 13 && this.content.length) {
					let formData = new FormData();
					window.console.log("user_id:",this.user.id)
					formData.append('room_id', this.currentId);
					formData.append('user_id',this.user.id);
					formData.append('content', this.content);
					
					var _this = this;
					this.$axios.post('/api/chat_in_chatroom', formData).then(function(response) {

					})
					this.$store.commit('SEND_MESSAGE', this.content);
					this.content = '';
				}
			}
		}
	};
</script>

<style lang="less" scoped>
	.text {
		height: 160px;
		border-top: solid 1px #ddd;

		textarea {
			padding: 10px;
			height: 100%;
			width: 100%;
			border: none;
			outline: none;
			font-family: "Micrsofot Yahei";
			resize: none;
		}
	}
</style>
