<template>
<br>
<div class="container">
  <Modal :alert="alert" @closeModal="alert = null"/>
  <form class="card" @submit.prevent>
  <h1>Vue http request</h1>
  <div class="form-control">
    <label for="name">Enter your to do!</label>
    <input type="text" id="name" placeholder="Enter to do" v-model.trim="newTodo">
  </div>
  <button class="btn" @click.enter="createTodo">Create to do</button>
  </form>
  <todoList :todos="toDos" @loadTodo="loadTodo" @deleted="remove" />
</div>
</template>

<script>
import todoList from '@/components/todoList'
import axios from 'axios'
import Modal from '@/components/ModalContent'
export default {
  name: 'App',
  components: {
    todoList,
    Modal
  },
  data () {
    return {
      newTodo: '',
      toDos: [],
      alert: null
    }
  },

  methods: {
    async createTodo () {
      const response = await fetch('https://vue-create-todo-default-rtdb.firebaseio.com/.json',
        {
          method: 'POST',
          headers: { 'Content-type': 'application/json' },
          body: JSON.stringify({
            todo: this.newTodo
          })
        })
      const fireBaseData = await response.json()
      this.toDos.push({
        todo: this.newTodo,
        id: fireBaseData.todo
      })
      this.newTodo = ''
    },

    async loadTodo () {
      if (this.newTodo !== '') {}
      /* eslint-disable */
      try {
        const { data } = await axios.get('https://vue-create-todo-default-rtdb.firebaseio.com/.json')
        if (data) {
          const response = Object.keys(data).map(key => {
            return {
              id: key,
              todo: data[key].todo
            }
          })
          this.toDos = response
          this.alert = {
            title: 'Ajoyib',
            text: 'Ma`lumotlar yuklandi baza bilan aloqa bor !',
            type: 'primary'
          }
        }
      } catch (error) {
        // console.log(e)
        this.alert = {
          title: 'Xato',
          text: 'Ma`lumotlar yuklanmadi baza bilan aloqa yo`q !',
          type: 'danger'
        }
      }
    },

    async remove (id) {
      this.toDos.splice(id, 1)
      try {
        await axios.delete(`https://vue-create-todo-default-rtdb.firebaseio.com/$(id).json`)
      } catch (error) {
        console.log(error)
      }
    }
  }

  // created () {
  //   this.loadTodo()
  //   console.log(this.toDos)
  // }

}
</script>
