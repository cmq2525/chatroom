<template>
	<div style="width:480px">
		<el-form>
			<el-form-item label="账户">
				<el-input v-model="username"></el-input>
			</el-form-item>
			<el-form-item label="密码">
				<el-input v-model="password" show-password></el-input>
			</el-form-item>
			<el-form-item>
				<el-button type="primary" @click="submitForm">登录</el-button>
				<el-button @click="resetForm">重置</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				username: "",
				password: ""
			}
		},
		methods: {
			submitForm() {
				//event.preventDefault();
				let formData = new FormData();
				formData.append('username', this.username);
				formData.append('password', this.password);


				var _this = this;
				this.$axios.post('/api/login', formData).then(function(response) {
					let msg = response.data.msg;
					if(msg == 'suc')
					{
						_this.$message({
							type: 'success',
							message: "登录成功！",
							showClose: true
						});
						window.console.log("login suc");
						_this.$forceUpdate();
					}
					else{
						_this.$message({
							type: 'error',
							message: msg,
							showClose: true
						});
						window.console.log(msg);
					}
					
				});
			},
			resetForm() {
				this.username = '';
				this.password = '';
			}
		}
	}
</script>
