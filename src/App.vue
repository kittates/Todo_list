<template>
    <div id="root">
		<div class="todo-container">
			<div class="todo-wrap">
				<MyHeader :addTodo="addTodo"/>
				<MyList 
                    :todos="todos" 
                    :reverseTodoDone="reverseTodoDone"
                    :deleteTodo="deleteTodo"
                />
				<MyFooter 
                    :competition="competition"
                    :clearDone="clearDone"
                    :todosIsAll="todosIsAll"
                />
			</div>
		</div>
	</div>
</template>

<script>
    import MyHeader from "./components/MyHeader.vue";
    import MyList from "./components/MyList.vue";
    import MyFooter from "./components/MyFooter.vue";

    export default {
        name: "App",
        components: {
            MyHeader,
            MyList,
            MyFooter
        },
        data() {
            return {
                todos: JSON.parse(localStorage.getItem("todos")) || [],
                competition: {
                    done: 0,
                    sum: 0
                }
            }
        },
        methods: {
            addTodo(todoObj) {
                this.todos.unshift(todoObj);
            },
            reverseTodoDone(id) {
                //也可以用forEach
                this.todos.find((element) => {
                    if(element.id===id) element.done = !element.done;
                });
            },
            deleteTodo(id) {
                let count = 0;
                this.todos.forEach((element,index) => {
                    if(element.id === id) {
                        count = index;
                        return;
                    } 
                })
                this.todos.splice(count,1);
                //也可以用filter,更方便
                // this.todos = this.todos.filter(element => element.id !== id)
            },
            clearDone() {
                //这里用filter就很方便
                this.todos = this.todos.filter(element => element.done ===false);
            },
            todosIsAll(newValue) {
                this.todos.forEach((element) => element.done = newValue);
            }
        },
        watch: {
            todos: {
                deep: true,
                immediate: true,
                handler(newValue) {
                    this.competition.sum = newValue.length;
                    //也可以使用reduce统计current的done值，然后与pre相加即可统计出总的已完成的任务个数
                    this.competition.done = newValue.filter(element => element.done===true).length;
                    //一旦todos发生改变，就重新写入
                    localStorage.setItem("todos",JSON.stringify(newValue));
                    
                }
            }
        }
    }
</script>

<style>
    /*base*/
	body {
		background: #fff;
	}
	.btn {
		display: inline-block;
		padding: 4px 12px;
		margin-bottom: 0;
		font-size: 14px;
		line-height: 20px;
		text-align: center;
		vertical-align: middle;
		cursor: pointer;
		box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
		border-radius: 4px;
	}
	.btn-danger {
		color: #fff;
		background-color: #da4f49;
		border: 1px solid #bd362f;
	}
	.btn-danger:hover {
		color: #fff;
		background-color: #bd362f;
	}
	.btn:focus {
		outline: none;
	}
	.todo-container {
		width: 600px;
		margin: 0 auto;
	}
	.todo-container .todo-wrap {
		padding: 10px;
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>