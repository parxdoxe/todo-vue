<template>
  <section class="todoapp">

      
			<header class="header">
				<h1>To do list</h1>
				<input class="new-todo" placeholder="What needs to be done?" autofocus v-model="newTodo" @keyup.enter="ajoutTab">
			</header>
			
			<section class="main" >
				
        <input @click="fleche = !fleche" id="toggle-all" class="toggle-all" type="checkbox" v-model="cliqueFleche"  >
        <label for="toggle-all">Mark all as complete</label>

				<ul  class="todo-list">
					
					<li class="todo" v-for="(todo,index) of groupeT" :key="index" :class="{'completed': todo.etat == true, 'editing': todo === edit}">

						<div class="view">
							<input class="toggle" type="checkbox" v-model="todo.etat" >
							<label @dblclick="cliqueEdit(todo)" >{{ todo.name }}</label>
							<button class="destroy" @click="deleteTodo(index)" ></button>
						</div>

						<input type="text" class="edit" v-model="todo.name" @keyup.enter="enterEdit">

					</li>
					
				</ul>

				
			</section>

			
			<footer class="footer">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong>{{ rest }}</strong> Task to do</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a :class="{selected: groupe == 'all'}" @click="groupe = 'all'" href="#/">All</a>
					</li>
					<li>
						<a :class="{selected: groupe == 'active'}" @click="groupe = 'active'"  href="#/active">Active</a>
					</li>
					<li>
						<a :class="{selected: groupe == 'completed'}" @click="groupe = 'completed'" href="#/completed">Completed</a>
					</li>
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed" v-show="clearBtn" @click="clearCompleted()">Clear completed</button>
			</footer>
  

		</section>
</template>

<script>


export default {
  
  data(){
    return {
      todos: [{name: '(Ajouter)' , etat: false}],
      newTodo: '' ,
      groupe: 'all',
      edit: null,
      fleche: false,
    }
    
  },
  methods: {
    ajoutTab () {
      this.todos.push({etat: false, name: this.newTodo,})
      this.newTodo = ''
    },
    deleteTodo (index){
      this.todos.splice(index,1)
    },
    clearCompleted () {
     this.todos = this.todos.filter(todo => !todo.etat ) // Etat = false, nouveau tableau avec valeur = false
    },
    cliqueEdit (todo) {
      this.edit = todo
    },
    enterEdit (){
      this.edit = null
    },
    
  },
  computed: {
    rest (){
      return this.todos.filter(todo => !todo.etat).length //Etat = false, création tableau avec les valeurs non complété
    },
    clearBtn () {
      return this.todos.filter(todo => todo.etat).length // Etat = true, création tableau avec valeur = false
    },
    groupeT (){
      if(this.groupe == 'active'){
        return this.todos.filter(todo => !todo.etat) //Etat = false
      } else if (this.groupe == 'completed'){
        return this.todos.filter(todo => todo.etat) //Etat = true
      } return this.todos
    },
    cliqueFleche (){
      if (this.fleche == true) {
       this.todos.forEach(todo => {
         todo.etat = true
       });
      } else if (this.fleche == false) {
        this.todos.forEach(todo => {
          todo.etat = false
        })
      }

      return this.todos
    }
  
  }
}
</script>

<style src="./app.css"></style>
