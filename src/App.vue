<template>
  <div class="container">
    <app-alert :alert="alert" @close="alert = null"></app-alert>
    <form class="card" @submit.prevent="createPerson">
      <h2>Работа с базой данных</h2>
      <div class="form-control">
        <label for="name">Введите имя</label>
        <input type="text" id="name" v-model.trim="name">
      </div>

      <button class="btn primary" :disabled="name.length === 0">Создать человека</button>
    </form>
    <app-loader v-if="isLoading"></app-loader>
    <app-people-list
      v-else
      :people="people"
      @load = "loadPeople"
      @remove = "removePerson"
    ></app-people-list>
  </div>
</template>

<script>
import axios from 'axios'
import AppPeopleList from '@/AppPeopleList'
import AppAlert from '@/AppAlert'
import AppLoader from '@/AppLoader'

export default {
  data () {
    return {
      name: '',
      people: [],
      alert: null,
      isLoading: false
    }
  },
  mounted () {
    this.loadPeople()
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

      this.people.push({
        firstName: this.name,
        id: personData.name
      })

      this.name = ''
    },
    async loadPeople () {
      this.isLoading = true
      try {
        const { data } = await axios.get('https://pet-vue-http-default-rtdb.europe-west1.firebasedatabase.app/people.json')

        if (!data) {
          throw new Error('Список людей пуст')
        }
        this.people = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      } catch (e) {
        this.alert = {
          title: 'Ошибка!',
          text: e.message,
          type: 'danger'
        }
      } finally {
        this.isLoading = false
      }
    },
    async removePerson (id) {
      try {
        await axios.delete(`https://pet-vue-http-default-rtdb.europe-west1.firebasedatabase.app/people/${id}.json`)
        const personName = this.people.find(person => person.id === id).firstName
        this.people = this.people.filter(person => person.id !== id)
        this.alert = {
          title: 'Успешно!',
          text: `Пользователь с именем "${personName}" был удален`,
          type: 'primary'
        }
      } catch (e) {
        this.alert = {
          title: 'Ошибка!',
          text: e.message,
          type: 'danger'
        }
      }
    }
  },
  components: {
    AppPeopleList,
    AppAlert,
    AppLoader
  }
}
</script>
