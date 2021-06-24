<template>
    <div class="question-box-container mt-5">
        <b-jumbotron >
            <template slot="lead">
               {{ currentQuestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                    :disabled = "isAnswered" 
                >
                    {{answer}}    
                </b-list-group-item>
            </b-list-group>

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || isAnswered"
            >
                Submit
            </b-button>
            <b-button 
                variant="success"
                @click="next"
                :disabled="!isAnswered"
            >
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

// To make our questions work, we don't just reference from HTML
// We have to pass data by JS also

<script>
    import _ from 'lodash';
    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                isAnswered: false
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer)
                return answers;
            }
        },
        watch: {
            currentQuestion: {
                immediate: true, // We added this option, call it, not only in changing point, but at start also
                handler() {
                    this.selectedIndex = null
                    this.isAnswered = false
                    this.shuffleAnswers()
                }   
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            submitAnswer() {
                let isCorrect = false

                if(this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }

                this.isAnswered = true

                this.increment(isCorrect)
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            answerClass(index) {
                let answerClass = ''
                if (!this.isAnswered && this.selectedIndex === index) {
                    answerClass = 'selected'
                } else if (this.isAnswered && this.correctIndex === index) {
                    answerClass = 'correct' 
                } else if (this.isAnswered && this.selectedIndex === index && this.correctIndex !== index) {
                    answerClass = 'incorrect'
                }

                return answerClass
            }
        
        }   

    }
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
        
    }
    .list-group-item {
        transition: all .01s;
    }
    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }
    .selected {
        background-color: lightblue;
    }
    .selected:hover {
        background-color: lightblue;
    }
    .correct {
        background-color: #27d484;
    }
    .correct:hover {
        background-color: #27d484;
    }
    .incorrect {
        background-color: tomato;
    }
    .incorrect:hover {
        background-color: tomato;
    }
    .btn {
        margin: 0 5px;
    }
</style>