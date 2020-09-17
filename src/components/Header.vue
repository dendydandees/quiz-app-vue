<template>
  <div>
    <b-nav>
      <b-nav-item active href="/"><b>Quiz App</b></b-nav-item>
      <b-nav-text disabled>Correct Answer: {{numCorrect}}/{{numTotal}}</b-nav-text>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>

<style scoped>

</style>
