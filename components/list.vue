<script>
import {mapGetters} from 'vuex'
export default {
    data(){
		return {
			x:[1,2,3],
		}
	},
	created(){
	},
	computed:{
		...mapGetters({
			filteredSessions:'filteredSessions',
			user:'user'
		}),
		
	}
};
</script>

<template>
<div class="list">
    <ul>
        <li v-for="item in filteredSessions" >
            <!-- <img class="avatar"  width="30" height="30" :alt="item.room.name" 
			:src="item.room.img"> -->
			<i class="el-icon-chat-dot-round"></i>
            <p class="name">聊天室-{{item.room.id}}</p>
        </li>
    </ul>
</div>
</template>

<style scoped lang="less">
.list {
    li {
        padding: 12px 15px;
        border-bottom: 1px solid #292C33;
        cursor: pointer;
        transition: background-color .1s;
		list-style: none;
        &:hover {
            background-color: rgba(255, 255, 255, 0.03);
        }
        &.active {
            background-color: rgba(255, 255, 255, 0.1);
        }
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
    }
}
</style>