<template>
  <div id="todo">
    <form>
      <label>
        New Todo:
        <input v-model="title" type="text"/>
        <button type="submit" @click.prevent="addTodo()">Add</button>
      </label>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <label>
        <input
          type="checkbox"
          v-model="todo.is_completed"
          @change="updateTodo(todo)">
            {{todo.title}}
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
  import { todosCollection } from './../firebase';

  export default {
    name: 'todo',
    data () {
      return {
        title: '',
        todos: []
      }
    },
    firestore() {
      return {
        todos: todosCollection.orderBy('created_at', 'desc')
      }
    },
    methods: {
      addTodo() {
        todosCollection.add({
          title: this.title,
          created_at: new Date(),
          is_completed: false
        })
        .then(function(docRef) {
          console.log("Document written with ID: ", docRef.id);
        })
        .catch(function(error) {
          console.error("Error adding document: ", error);
        });
        this.title = '';
      },
      updateTodo(todo) {
        todosCollection.doc(todo.id).update({...todo})
        .then(function() {
          console.log("Updated document with ID: ", todo.id);
        })
        .catch(function(error) {
          console.error("Error updating document: ", error);
        });
      }
    }
  }
</script>

<style scoped>

</style>
