<template>
<div class="card">
    <header>
        <img class="avatar" width="40" height="40" :alt="user.name" 
		:src="get_avatar(user.name)">
        <p class="name">{{user.name}}</p>
    </header>
    <footer>
        <input class="search" type="text" placeholder="search user..." @keyup="search">
    </footer>
</div>
</template>
<script>
import {mapGetters} from 'vuex'
export default {
	data() {
		return {
		};
	},
	computed:{
		...mapGetters({
			user:'user',
			avatars:'avatars'
		}),
		
	},
	methods:{
		search(e){
			this.$store.commit('SET_FILTER_KEY',e.target.value)
		},
		get_avatar (name){
			let img = this.$axios.defaults.baseURL + this.avatars[name];
			if(!img){
				let _this = this;
				this.$axios.get('api/get_avatar',{
					'name':name
				}).then(function(response){
					img = response.data.img;
					_this.$store.commit('REFRESH_AVATAR',name,img);
					_this.$forceUpdate();
				});
			}
			return img;
		}
		
	},
	created(){
		window.console.log('---------');
		
	}

};
</script>



<style scoped lang="less">
.card {
    padding: 12px;
    border-bottom: solid 1px #24272C;

    footer {
        margin-top: 10px;
    }

    .avatar, .name {
        vertical-align: middle;
    }
    .avatar {
        border-radius: 2px;
    }
    .name {
        display: inline-block;
        margin: 0 0 0 15px;
        font-size: 16px;
    }
    .search {
        padding: 0 10px;
        width: 100%;
        font-size: 12px;
        color: #fff;
        height: 30px;
        line-height: 30px;
        border: solid 1px #3a3a3a;
        border-radius: 4px;
        outline: none;
        background-color: #26292E;
    }
}
</style>