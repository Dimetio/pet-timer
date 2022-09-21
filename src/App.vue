<template>
  <Counter 
    :year="2022"
    :month="8"
    :day="21"
    :hour="21"
    :minute="51"
    :second="40"
    :millisecond="10"
  />
</template>

<script>
import Counter from '@/components/Counter.vue'
export default {
components: {
  Counter
}
}
</script>

<style lang="scss" scoped>

</style>