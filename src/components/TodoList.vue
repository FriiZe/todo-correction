<script>
    export default {
        name: "TodoList",
        props: ["todoList", "shouldDisplayOnlyDoneTodos"],
        methods: {
            deleteTodo(index) {
                this.$emit("deleteTodo", index)
            },
            doneTodo(index) {
                this.$emit("doneTodo", index)
            },
            selectTodo(index) {
                console.log('select', index)
                this.$emit("selectTodo", index)
            },
            editTodo(index) { 
                this.$emit("editTodo", index)
            }
        },
        computed: {
            filteredTodoList() {
                return !!this.shouldDisplayOnlyDoneTodos ? this.todoList.filter((todo) => todo.status === 'DONE') : this.todoList
            },
        }
    }
</script>
<template>
    <div>
        <div v-for="(todo, index) in filteredTodoList" :style="{
            borderStyle:'solid',
            borderColor: todo.status === 'DONE' ? 'green' : 'red',
            backgroundColor: todo.isSelected ? 'grey' : 'white',
            width: '40%',
            marginTop: '10px'

        }" @click="selectTodo(index)">
        <div :style="{ display: 'flex'}">
            <p>{{ todo.owner }} doit faire {{ todo.text}} en {{ todo.hours }} heures</p>
            <button @click="deleteTodo(index)">Supprimer</button>
            <button @click="doneTodo(index)" v-if="todo.status !== 'DONE'">C'est fait !</button>
            <button @click="editTodo(index)">Éditer</button>
        </div>
        </div>
    </div>
</template>