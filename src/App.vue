<!--/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//
//    HTML
//
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////-->


<template>
    
    <div class="container">
        <h1>ToDo-List</h1>

        <div class="wrapper">

            <!-- NEW TODO -->
            <div class="add-todo">
                <h2>Add New</h2>
                <div class="add-todo-field">
                    <input @keypress.enter="addNewActive" type="text" v-model="todoInput" placeholder="What needs to be done?">
                    <button class="add-button" @click.prevent="addNewActive"><i class="fa-solid fa-plus"></i></button>
                </div>
            </div>

            <!-- ACTIVE TODO -->
            <div class="todos-container">
                <h2 v-if="activeTodo.length > 0">Remaining ToDo's: <span v-if="count > 0">{{ count }}</span></h2>
                <h2 class="no-todos" v-if="activeTodo.length < 1">No ToDo's added yet.</h2>
                <div v-else v-for="(listItem, idx) in activeTodo" :key="listItem">
                    <p>
                        {{ listItem.todoItem }}
                        <button @click="removeActive(idx)">
                            <i class="fa-solid fa-trash-can delete-todo"></i>
                        </button>
                    </p>
                </div>
            </div>

            <!-- DELETE ALL TODOS -->
            <div class="delete-todo-all">
                <button @click.prevent="deleteAll">Delete All</button>
            </div>

        </div>

    </div>

</template>



<!--/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//
//    JAVASCRIPT
//
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////-->


<script>

    export default {

    /////////////////////////////////
    // INIT
    /////////////////////////////////

      data() {
        return {
            todoInput: "",
            activeTodo: [],
        };
      },


      /////////////////////////////////
      // EVENTS
      /////////////////////////////////

      mounted() {

        let activeTodo = localStorage.getItem("activeTodo")
        if (activeTodo) { this.activeTodo = JSON.parse(activeTodo); }
      },

      /////////////////////////////////
      // METHODS
      /////////////////////////////////

      computed: {

        count() { return this.activeTodo.length; }
      },

      methods: {

          addNewActive() {

            if (this.todoInput.trim() === "") return;

            this.activeTodo.push({ "todoItem" : this.todoInput });
            this.todoInput = "";
            localStorage.setItem("activeTodo", JSON.stringify(this.activeTodo))
        },

        removeActive(idx) { 

            this.activeTodo.splice(idx, 1); 
            localStorage.setItem("activeTodo", JSON.stringify(this.activeTodo))
        },

        deleteAll() {

            this.activeTodo = [];
            localStorage.setItem("activeTodo", JSON.stringify(this.activeTodo))
        }

      },

      
  }; // end export

  </script>
