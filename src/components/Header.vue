<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled> <b> Fancy quizz app </b> </b-nav-item>
      <b-nav-item disabled> Correct answers {{ correctAnswers }} / {{ totalAnswers }} </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'totalAnswers',
    'correctAnswers'
  ]
}
</script>
