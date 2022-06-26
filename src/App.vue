<template>
    <div id="app">
        <HeaderQuiz :numCorrect="numCorrect" :numTotal="numTotal" />
        <b-container class="bv-example-row">
            <b-row>
                <b-col sm="6">
                    <!-- v-if bên dưới vì có trường hợp Component QuestionBox render trước khi call xong API nên bị undefined -->
                    <QuestionBox v-if="questions.length" :currentQuestion="questions[index]" :next="next"
                        :increment="increment" />
                </b-col>
            </b-row>
        </b-container>

    </div>
</template>

<script>
import HeaderQuiz from './components/HeaderQuiz.vue'
import QuestionBox from './components/QuestionBox.vue'


export default {
    name: 'App',
    components: {
        QuestionBox,
        HeaderQuiz
    },
    data() {
        return {
            questions: [],
            index: 0,
            numCorrect: 0,
            numTotal: 0,
        }
    },
    methods: {
        next: function () {
            this.index++;
        },
        increment(isCorrect) {
            if (isCorrect) {
                this.numCorrect++
            }
            this.numTotal++;
        }
    },
    mounted: function () {
        fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
            method: 'get'
        })
            .then((response) => {
                return response.json();
            })
            .then((jsonData) => {
                this.questions = jsonData.results;
            })
    }
}
</script>

<style>
</style>
