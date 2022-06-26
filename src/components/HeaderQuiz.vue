<template>
    <div class="header">
        <b-nav tabs>
            <b-nav-item disabled><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>

        </b-nav>
    </div>
</template>

<script>
export default {
    name: 'HeadQuiz',
    props: [
        'numCorrect',
        'numTotal'
    ]

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
