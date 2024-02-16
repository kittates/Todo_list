<template>
    <div class="todo-header">
		<input type="text" placeholder="请输入你的任务名称，按回车键确认" ref="addInfo" @keyup.enter="add"/>
	</div>
</template>

<script>
    import {nanoid} from "nanoid"
    export default {
        name: "MyHeader",
        methods: {
            add() {
                if(this.$refs.addInfo.value.trim() == "") return alert("请输入任务！！！");
                //也可以使用e获取input's value,也可以使用v-model双向绑定
                let todoObj = {
                    id: nanoid(),
                    title: this.$refs.addInfo.value,
                    done: false
                }
                this.addTodo(todoObj);
                this.$refs.addInfo.value = "";
            }
        },
        props: ["addTodo"],
    }
</script>

<style scoped>
    /*header*/
	.todo-header input {
		width: 560px;
		height: 28px;
		font-size: 14px;
		border: 1px solid #ccc;
		border-radius: 4px;
		padding: 4px 7px;
	}

	.todo-header input:focus {
		outline: none;
		border-color: rgba(82, 168, 236, 0.8);
		box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
	}
</style>