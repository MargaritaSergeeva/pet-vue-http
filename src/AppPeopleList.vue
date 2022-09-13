<template>
  <div v-if="people.length">
    <div v-for="person in people" :key="person.id" class="card row">
      <h3>{{ person.firstName }}</h3>
      <button class="btn danger" @click="$emit('remove', person.id )">Удалить</button>
    </div>
  </div>
  <div v-else class="card">
    <h4>Данных пока нет</h4>
    <button class="btn" @click="$emit('load')">Загрузить список людей</button>
  </div>
</template>

<script>
export default {
  emits: ['load', 'remove'],
  props: ['people']
}
</script>

<style>
  .row {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
