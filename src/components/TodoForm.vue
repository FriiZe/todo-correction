<script>
    export default {
        name: "TodoForm",
        props: ["todoToEdit", "existingTodos"],
        data() {
            return {
                addingTodo: {
                    text: "",
                    hours: 0,
                    owner: "",
                    status: "TODO",
                },
                isEditingMode: false,
            }
        },
        watch: {
            todoToEdit() {
                this.isEditingMode = true
                this.addingTodo = {
                    ...this.todoToEdit
                }
            }
        },
        methods: {
            handleError() { 
                if (!this.addingTodo.text || !this.addingTodo.owner || !Number.isInteger(this.addingTodo.hours)) {
                    alert("Veuillez remplir tous les champs")
                    return 1
                }
                const ownerTodos = this.existingTodos.filter((todo) => todo.owner === this.addingTodo.owner)
                let usedHours = 0;
                for (const todo of ownerTodos) {
                    usedHours += todo.hours
                }
                if (
                    ownerTodos.length >= 3 || usedHours >= 10
                ) {
                    alert("Ce responsable est trop occupé")
                    return 1
                }
                return 0
            },
            addTodo() {
                if (this.isEditingMode) this.isEditingMode = false
                const hasError = this.handleError()
                if (hasError) return
                this.$emit("addTodo", this.addingTodo)
                this.addingTodo = {
                    text: "",
                    hours: 0,
                    owner: "",
                    status: "TODO",
                }
            }
        }
    }
</script>
<template>
    <div>
        <input type="text" v-model="addingTodo.text">
        <input type="number" v-model="addingTodo.hours">
        <select name="owner" id="" v-model="addingTodo.owner">
            <option value="">--Please choose an option--</option>
            <option value="Pierre">Pierre</option>
            <option value="Paul">Paul</option>
        </select>
        <button @click="addTodo">{{isEditingMode ? 'Éditer' : 'Ajouter'}}</button>
    </div>
</template>