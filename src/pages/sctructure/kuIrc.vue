<template>
  <div>
    <div class="headline">
      Комунальна установа "Інклюзивно-ресурсний центр"
    </div>
    <app-person v-for="person in persons" :person="person" :key="person.order"/>
  </div>
</template>

<script>
  import fetchAndLoad from '../../mixins/fetchAndLoad';
  import AppPerson from '../../components/AppPerson';

  export default {
    name: "kuIrc",
    mixins: [fetchAndLoad],
    components: {AppPerson},
    data() {
      return {
        persons: []
      }
    },
    created() {
      this.$fetch('structureIRC').then(persons => Object.assign(this.$data, {persons}))
    }
  }
</script>

<style lang="scss" scoped>
</style>
