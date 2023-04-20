<script>
    import TodoForm from '../components/TodoForm.vue'; 
    import TodoList from '../components/TodoList.vue';
    export default {
        name: 'TodoPage',
        components: {
            TodoForm,
            TodoList
        },
        data() {
            return {
                todoList: [],
                shouldDisplayOnlyDoneTodos: false,
                todoToEdit: null,
            }
        },
        methods: {
            todoHasBeenAdded(newTodo) {
                this.todoList = [
                    ...this.todoList,
                    newTodo
                ]
            },
            deleteTodo(index) {
                this.todoList = [
                    ...this.todoList.slice(0, index),
                    ...this.todoList.slice(index + 1, this.todoList.length)
                ]
            },
            doneTodo(index) {
                this.todoList = [
                    ...this.todoList.slice(0, index),
                    {
                        ...this.todoList[index],
                        status: 'DONE'
                    },
                    ...this.todoList.slice(index + 1, this.todoList.length)
                ]
            },
            editTodo(index) {
                this.todoToEdit = this.todoList[index]
                this.deleteTodo(index)
            },
            selectTodo(index) {
                this.todoList = [
                    ...this.todoList.slice(0, index),
                    {
                        ...this.todoList[index],
                        isSelected: !this.todoList[index].isSelected,
                    },
                    ...this.todoList.slice(index + 1, this.todoList.length)
                ]
            },
            deleteSelectedTodos(arg) {
                console.log(arg)
                this.todoList = [
                    ...this.todoList.filter((todo) => !todo.isSelected)
                ]
            }
        }
    }
</script>
<template>
    <h1>Ceci est la page des todos</h1>
    <div :style="{
        borderStyle:'solid',
        width: '20%',
    }">
        <h2>Filtres</h2>
        <input type="checkbox" name="" id="displayOnlyDoneTodos" v-model="shouldDisplayOnlyDoneTodos">
        <label for="displayOnlyDoneTodos">Afficher uniquement les todos terminées</label>
    </div>
    <TodoForm @addTodo="todoHasBeenAdded" :todoToEdit="todoToEdit" :existingTodos="todoList"/>
    <TodoList :todoList="todoList" :shouldDisplayOnlyDoneTodos="shouldDisplayOnlyDoneTodos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" @selectTodo="selectTodo" @editTodo="editTodo"/>

    <div>
        <p>Il y a {{ todoList.length }} taches au total</p>
        <p>Il y a {{ todoList.filter((todo) => todo.status === 'TODO').length }} taches a faire</p>
        <p>Il y a {{ todoList.filter((todo) => todo.status === 'DONE').length }} taches effectués</p>
        <p>Il y a {{ todoList.filter((todo) => !!todo.isSelected).length }} taches selectionnées</p>
    </div>
    <button @click="deleteSelectedTodos" v-if="todoList.some((todo) => todo.isSelected)">Suprimer les todos sélectionnées</button>
</template>
<style></style>


