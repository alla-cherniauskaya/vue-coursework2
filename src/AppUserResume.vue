<template>
  <div class="card card-w70">
    <div v-if="userData.length">
      <div  v-for="element in userData" :key="element.id">
        <h1 v-if="element.blockType === 'title'"> {{element.value}}</h1>
        <div v-if="element.blockType === 'avatar'" class="avatar">
          <img :src="element.value">
        </div>
        <h2 v-if="element.blockType === 'subtitle'"> {{element.value}}</h2>
        <p v-if="element.blockType === 'text'">
          {{element.value}}
        </p>
      </div>
     
    </div>
   
     <h3 v-else> Добавьте первый блок, чтобы увидеть результат</h3>
   </div>
</template>

<script>

export default {
  props: {
    userData: {
      type: Array,
    }
  }
}

</script>

<style>

</style>