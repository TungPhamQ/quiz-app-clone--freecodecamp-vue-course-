<template>
    <div class="question-box">
        <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
            <template #lead>
                {{ currentQuestion.question }}
            </template>
            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in shuffledAnswers" :key="index" @click="selectAnswer(index)"
                    :class="answerClass(index)">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit
            </b-button>
            <b-button variant="success" @click="next">Next</b-button>
        </b-jumbotron>

    </div>
</template>

<script>
import _ from 'lodash'
export default {
    name: 'QuestionBox',
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.shuffleAnswer();
                this.answered = false;
            }
        }

        // () {
        //     this.selectedIndex = null;
        //     this.shuffleAnswer();
        // }
    },
    methods: {
        selectAnswer(index) {
            console.log(index);
            this.selectedIndex = index;
        },
        shuffleAnswer() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false;

            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect)
        },
        answerClass(index) {
            let answerClass = '';

            if (!this.answered && this.selectedIndex === index) {
                answerClass = 'selected';
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct';
            } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'incorrect';
            }
            return answerClass;
        }
    },
    // mounted() {
    //     this.shuffledAnswers();
    //      //Cách 1: vì thông thường hàm shuffle thực hiện sau khi render nên vẫn hiển thị ra answers chưa shuffle
    //      // sử dụng hàm mount để gọi hàm shuffle ngay khi render nên sẽ render phần data đẫ shuffle

    //     //Cách 2:
    //     // trong watch gọi thuộc tính immediate để có thể gọi ngay lập tức
    // }



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list-group {
    margin-bottom: 15px;
}

.list-group-item:hover {
    background: #eee;
    cursor: pointer;
}

.btn {
    margin: 0 5px;
}

.selected {
    background: lightblue;
}

.correct {
    background: lightgreen;
}

.incorrect {
    background: red;
}
</style>
