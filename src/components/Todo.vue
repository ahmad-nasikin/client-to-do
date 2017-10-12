<template>
  <div class="container">
    <div class="row">
      <div class="col-md-4 col-md-offset-4">
        <form method="post" @submit.prevent="postTodo()">
            <input type="text" name="todo" placeholder="input todo " v-model="post.task">
            <button name="button">add todo</button>
          </form>
          <table>
            <thead>
              <tr>
                <td>Todo</td>
                <td>Done</td>
                <td>Actions</td>
              </tr>
            </thead>
            <tbody>
              <tr v-for="todo in todos">
                <td>{{ todo.task }}</td>
                <div class="ui toggle checkbox">
                  <input class="ui toggle checkbox" type="checkbox" @click="editStatusTodo(todo)">
                  <label><b>{{ todo.status }}</b></label>
                </div>
                <td><button type="submit" @click="deleteTodo(todo._id)">hapus</button></td>
              </tr>
            </tbody>
          </table>
      </div>
        <div>
          <button @click="logout">Logout</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      todos: [],
      post: {
        task: ''
      }
    }
  },
  methods: {
    getTodo () {
      var self = this
      this.$http.get('/todo', {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(result => {
        console.log('result', result.data)
        self.todos = result.data
      })
      .catch(err => {
        console.error(err)
      })
    },
    postTodo () {
      var self = this
      this.$http.post('/todo', {
        task: self.post.task
      }, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(result => {
        self.todos.push(result.data)
      })
      .catch(err => {
        console.error(err)
      })
    },
    editStatusTodo (data, index) {
      let status = ''
      if (data.status === 'done') {
        status = 'undone'
      } else {
        status = 'done'
      }
      let self = this
      this.$http.put(`todo/${data._id}`, {
        status: status
      }, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(response => {
        self.todos[index].status = status
      })
      .catch(err => {
        console.error(err)
      })
    },
    deleteTodo (data, index) {
      var self = this
      this.$http.delete(`/todo/${data}`, {
        headers: {
          token: localStorage.getItem('token')
        }
      })
      .then(response => {
        self.todos.splice(index, 1)
      })
    },
    login () {

    },
    logout () {
      localStorage.clear()
      this.$router.push('/')
    }
  },
  created () {
    if (localStorage.getItem('token')) {
      this.getTodo()
    } else {
      this.$router.push('/')
    }
  }
}
</script>
