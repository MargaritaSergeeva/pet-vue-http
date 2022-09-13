<template>
  <div class="container">
    <form class="card" @submit.prevent="createPerson">
      <h2>Работа с базой данных</h2>
      <div class="form-control">
        <label for="name">Введите имя</label>
        <input type="text" id="name" v-model.trim="name">
      </div>

      <button class="btn primary" :disabled="name.length === 0">Создать человека</button>
    </form>
    <app-people-list
      :people="people"
      @load = "loadPeople"
    ></app-people-list>
  </div>
</template>

<script>
import AppPeopleList from '@/AppPeopleList'

export default {
  data () {
    return {
      name: '',
      people: []
    }
  },
  methods: {
    async createPerson () {
      const response = await fetch('https://pet-vue-http-default-rtdb.europe-west1.firebasedatabase.app/people.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          firstName: this.name
        })
      })

      const personData = await response.json()
      console.log(personData)

      this.name = ''
    }
  },
  components: {
    AppPeopleList
  }
}
</script>
