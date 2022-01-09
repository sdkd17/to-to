<template>
  <div class="container">
    <h1 class="header">
      To Do App
    </h1>
    <div class="form">
      <form>
        <label> Add Task </label>
        <input v-model="task"> 
        <button @click.prevent="addTask"> 
          AddTask 
        </button>
      </form>
    </div>
    <div class="buttons-group">  
      <button @click.prevent="showCompleted"> 
        Completed 
      </button>
      <button @click.prevent="showUncompleted"> 
        Active 
      </button>
      <button @click.prevent="showAll"> 
        All 
      </button>
    </div>
    <div class="main">
      <div
        v-for="t in tasks" 
        v-show="showTask(t)"
        :key="t.id"
        :class="{'background-completed': t.completed}"
        class="main-container"
      > 
        <div class="main-left">
          <small> Created: {{ t.date.toLocaleString() }} </small>
          <h3 v-show="!t.edit"> 
            {{ t.title }}
          </h3>
          <div>
            <input 
              v-show="t.edit" 
              v-model="t.title"
            >
          </div>
        </div>
        
        <button 
          v-show="!t.completed" 
          class="main-right2 button-completed"
          @click="completeTask(t.id)"
        >
          Completed
        </button>
        <button
          :class="{'active-edit': t.edit}"
          class="main-right1"
          @click="editTask(t.id)"
        >
          Edit
        </button>
        <button
          class="main-right button-delete"
          @click="deleteTask(t.id)"
        > 
          Delete 
        </button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      task: '',
      tasksids: 0,
      tasks: [],
      show: 0 // 0 -> all, 1 -> completed, 2 -> uncompleted
    }
  },
  methods: {
    addTask: function() {
      // console.log([this.tasks.length+1, this.task])
      if (!this.tasks.find(t => t.title === this.task)){
        this.tasks.push({'id':this.tasksids, 'title':this.task, 'completed': false, 'date': new Date(), 'edit':false});
        this.tasksids += 1;
        this.task = '';
      } else {
        alert("The task " + this.task + " already exists.");
      }
    },
    completeTask: function(id) {
      let t = this.tasks.find(t => t.id === id);
      t.completed = true;
    },
    editTask: function (id) {
      let t = this.tasks.find(t => t.id === id);
      t.edit = !t.edit;
    },
    deleteTask: function(id) {
      if (confirm("Are you Sure?")) {
        const index = this.tasks.findIndex(t => t.id === id);
        this.tasks.splice(index,1);
      }
    },
    showCompleted: function() {
      this.show = 1
    },
    showUncompleted: function() {
      this.show = 2
    },
    showAll: function() {
      this.show = 0
    },
    showTask: function(t) {
      return (t.completed && this.show === 1) || (!t.completed && this.show === 2) || this.show === 0
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  display: grid;
  grid-template-columns: [first] 5% [main] auto [last] 5%;
  grid-template-rows: [header] 50px [form] 50px [buttons] 50px [body] auto [footer] 30px;
  place-items:  center;
  background-color: rgb(76, 76, 125);
}

.header {
  grid-column-start: main;
  grid-row-start: header;
  
  margin: 5px;
}

.form {
  grid-column-start: main;
  grid-row-start: form;

  margin: 10px;
}

.buttons-group {
  grid-column-start: main;
  grid-row-start: buttons;
}

.main {
  grid-column-start: main;
  grid-row-start: body;

  place-self: stretch;
}

.main-container {
  display: grid;
  grid-template-columns: [left] 65% [right2] auto [right1] auto [right] auto;
  /*grid-template-rows: [top] 50px [main-body] auto;*/
  /*place-items: center;*/

  padding: 5px;
  margin: 5px;
  border:  solid 1px rgb(0,0,255);
  border-radius: 10px;
  background-color: rgb(181, 181, 199);
  box-shadow: 5px 5px 5px grey;
}

.main-left {
  grid-column-start: left;
  margin-left: 5px;
}

.main-right2 {
   grid-column-start: right2; 
}

.main-right1 {
  grid-column-start: right1;
  /*grid-row-start: main-body;*/
}

.main-right {
  grid-column-start: right;
  /*grid-row-start: main-body;*/

}

button {
  background-color: rgb(181, 181, 199);
  color: black;
  border: 1px solid rgb(0, 0, 255); 
  border-radius: 10px;
  margin: 5px;
  padding: 10px;
}

button:hover {
  background-color: rgb(0, 0, 255);
  color: gray;
  box-shadow: 5px 5px 5px grey;
}


.button-delete {
  border-color: rgb(181, 53, 53);
}
.button-delete:hover {
  background-color: rgb(181, 53, 53);
  color:  black;
}

.button-completed {
  border-color: rgb(39, 130, 38); 
}
.button-completed:hover {
  background-color: rgb(39, 130, 38);
  color:  black;
}

.active-edit {
  background-color: rgb(181, 53, 53);
  color:  black;
}

.background-completed {
  background-color: rgb(39, 130, 38);
}

</style>
