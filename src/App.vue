<template>
  <div class="app">
    <h1>📝 Vue Todo App</h1>
    
    <div class="input-section">
      <input 
        v-model="newTodo" 
        @keyup.enter="addTodo"
        placeholder="What needs to be done?"
        class="todo-input"
      />
      <button @click="addTodo" class="add-btn">Add</button>
    </div>
    
    <div class="filters">
      <button 
        :class="{ active: filter === 'all' }"
        @click="filter = 'all'"
      >All</button>
      <button 
        :class="{ active: filter === 'active' }"
        @click="filter = 'active'"
      >Active</button>
      <button 
        :class="{ active: filter === 'completed' }"
        @click="filter = 'completed'"
      >Completed</button>
    </div>
    
    <ul class="todo-list">
      <li 
        v-for="todo in filteredTodos" 
        :key="todo.id"
        :class="{ completed: todo.completed }"
        class="todo-item"
      >
        <input 
          type="checkbox" 
          v-model="todo.completed"
          class="todo-checkbox"
        />
        <span class="todo-text">{{ todo.text }}</span>
        <button @click="removeTodo(todo.id)" class="delete-btn">×</button>
      </li>
    </ul>
    
    <div class="stats" v-if="todos.length">
      <span>{{ activeCount }} remaining</span>
      <button 
        v-if="completedCount > 0"
        @click="clearCompleted"
        class="clear-btn"
      >Clear completed</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos') || '[]'),
      newTodo: '',
      filter: 'all',
      nextId: Date.now()
    }
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'active') return this.todos.filter(t => !t.completed)
      if (this.filter === 'completed') return this.todos.filter(t => t.completed)
      return this.todos
    },
    activeCount() {
      return this.todos.filter(t => !t.completed).length
    },
    completedCount() {
      return this.todos.filter(t => t.completed).length
    }
  },
  watch: {
    todos: {
      handler(val) {
        localStorage.setItem('todos', JSON.stringify(val))
      },
      deep: true
    }
  },
  methods: {
    addTodo() {
      const text = this.newTodo.trim()
      if (!text) return
      this.todos.push({
        id: this.nextId++,
        text,
        completed: false
      })
      this.newTodo = ''
    },
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    },
    clearCompleted() {
      this.todos = this.todos.filter(t => !t.completed)
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: #f5f5f5;
  color: #333;
}

.app {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #42b883;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.todo-input {
  flex: 1;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
  transition: border-color 0.2s;
}

.todo-input:focus {
  border-color: #42b883;
}

.add-btn {
  padding: 12px 20px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.2s;
}

.add-btn:hover {
  background: #369970;
}

.filters {
  display: flex;
  gap: 8px;
  margin-bottom: 15px;
}

.filters button {
  padding: 8px 16px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.filters button.active {
  background: #42b883;
  color: white;
  border-color: #42b883;
}

.todo-list {
  list-style: none;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 12px;
  background: white;
  border-radius: 8px;
  margin-bottom: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  transition: all 0.2s;
}

.todo-item:hover {
  box-shadow: 0 2px 6px rgba(0,0,0,0.15);
}

.todo-checkbox {
  margin-right: 12px;
  width: 20px;
  height: 20px;
  cursor: pointer;
}

.todo-text {
  flex: 1;
  font-size: 16px;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #999;
}

.delete-btn {
  background: none;
  border: none;
  font-size: 20px;
  color: #999;
  cursor: pointer;
  padding: 0 8px;
  transition: color 0.2s;
}

.delete-btn:hover {
  color: #e74c3c;
}

.stats {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  color: #666;
  font-size: 14px;
}

.clear-btn {
  background: none;
  border: none;
  color: #e74c3c;
  cursor: pointer;
  font-size: 14px;
}

.clear-btn:hover {
  text-decoration: underline;
}
</style>
