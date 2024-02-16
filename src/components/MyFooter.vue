<template>
    <div class="todo-footer" v-show="competition.sum">
		<label>
			<input type="checkbox" v-model="isAll" @change="checkAll"/>
		</label>
		<span>
			<span>已完成{{competition.done}}</span> / 全部{{competition.sum}}
		</span>
		<button class="btn btn-danger" @click="clearDone">清除已完成任务</button>
        <div class="photoBox" ref="photoBox">
            <p>咋还有这么多任务:(</p>
            <img src="../assets/shushu1.jpg" alt="">
        </div>
	</div>
</template>

<script>
    export default {
        name: "MyFooter",
        props: ["competition","clearDone","todosIsAll"],
        data() {
            return {
                isAll: false,
            }
        }, 
        watch: {
            competition: {
                deep: true,
                immediate: true,
                handler(newValue) {
                    // if(newValue.done === this.competition.sum) this.isAll = true;
                    newValue.done === this.competition.sum && newValue.done >0 ? this.isAll=true:this.isAll=false;
                }
            },
        },
        methods: {
            checkAll(e) {
                this.todosIsAll(e.target.checked);
            }
        },
    }
</script>

<style scoped>
    /*footer*/
	.todo-footer {
		height: 40px;
		line-height: 40px;
		padding-left: 6px;
		margin-top: 5px;
	}

	.todo-footer label {
		display: inline-block;
		margin-right: 20px;
		cursor: pointer;
	}

	.todo-footer label input {
		position: relative;
		top: -1px;
		vertical-align: middle;
		margin-right: 5px;
	}

	.todo-footer button {
		float: right;
		margin-top: 5px;
	}
    .photoBox {
        display: block;
        margin-top: 20px;
        margin: 20px calc(50% - 400px);
        width: 800px;
        height: 500px;  
         
    }
    .photoBox img {
        width: 100%;
        height: 100%;
        border-radius: 20px;    
    }
    .photoBox p {
        display: block;
        margin:auto;   
        width: 23%;
        font-size: 20px;
        font-weight: 700;
    }
</style>