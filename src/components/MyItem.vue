<template>
    <li>
		<label>
			<input type="checkbox" :checked="todoItem.done" @click="showId(todoItem.id)" />
			<!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
			<!-- <input type="checkbox" v-model="todo.done"/> -->
			<span v-show="!isEdit">{{todoItem.title}}</span> 
            <span v-show="isEdit">
                <input type="text" class="modifyInput" 
                    :value="todoItem.title" ref="modifyInput" 
                    @blur="Update(todoItem.id)" 
                    @keydown.enter="Update(todoItem.id)"
                >
            </span>
		</label>
		<button class="btn btn-danger" @click="deleteItem(todoItem.id)">删除</button>
		<button class="btn btn-modify" @click="modify(todoItem.id)">编辑</button>
	</li>
</template>

<script>
    export default {
        name: "MyItem",
        props: ["todoItem"],
        data() {
            return {
                isEdit: false,
            }
        },
        methods: {
            showId(id) {
                //通知App将对应id的todo项中的done属性取反
                this.$bus.$emit("reverseTodoDone",id);
            },
            deleteItem(id) {
                if(confirm("确定要删除？")) {
                    this.$bus.$emit("deleteTodo",id);
                }
            },
            modify() {
                this.isEdit = true;
                this.$nextTick(() => {
                    this.$refs.modifyInput.focus();
                });
            },
            //失焦和enter时触发
            Update(id) {
                if(this.isEdit) {
                    this.isEdit = false;    //这样enter触发后将isEdit变为false,之后的blur就不会再次执行了
                    let info = this.$refs.modifyInput.value.trim();
                    if(info==="") return alert("内容不能为空");
                    this.$bus.$emit("modifyTodo",[id,info]);
                    
                }
            }
        },
    }
</script>

<style scoped>
    /*item*/
	li {
		list-style: none;
		height: 36px;
		line-height: 36px;
		padding: 0 5px;
		border-bottom: 1px solid #ddd;
	}

	li label {
		float: left;
		cursor: pointer;
	}

	li label li input {
		vertical-align: middle;
		margin-right: 6px;
		position: relative;
		top: -1px;
	}

	li button {
		float: right;
		display: none;
		margin-top: 3px;
	}

	li:before {
		content: initial;
	}

	li:last-child {
		border-bottom: none;
	}

	li:hover{
		background-color: #ddd;
	}
	
	li:hover button{
		display: block;
	}
    li .modifyInput {
        outline: none;
        border: .5px solid #ccc2c2;
        border-radius: 5px;
        text-indent: 2px;
        width: 400px;
        height: 25px;
        font-size: 14px;
    }
    li .modifyInput:focus {
        border-color: rgba(82, 168, 236, 0.8);
        box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
    }
</style>