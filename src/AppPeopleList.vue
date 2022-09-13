<template>
  <div v-if="people.length">
    <div v-for="person in people" :key="person" class="card">
      <h3>{{ person }}</h3>
    </div>
  </div>
  <div v-else class="card">
    <h4>Данных пока нет</h4>
    <button class="btn" @click="$emit('load')">Загрузить список людей</button>
  </div>
</template>

<script>
export default {
  emits: ['load'],
  props: ['people']
}
</script>
