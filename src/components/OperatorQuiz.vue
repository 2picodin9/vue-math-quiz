<template>
  <div>
      <div v-if="isQuizStarted">
        <h4>{{ operandLeft }} {{ operator }} {{ operandRight }}</h4>
      </div>
      <button v-for="(answer, index) of answers" :key="index" @click="selectAnswer(answer)">{{ answer }}</button>
      <button v-if="!isQuizStarted" @click="startQuiz">Start</button>
      <button @click="$emit('onBack')">Back</button>
  </div>
</template>

<script>
export default {
    props: ['operator'],
    data () {
        return {
            isQuizStarted: false,
            operandLeft: null,
            operandRight: null,
            answers: [],
            correctAnswer: null
        }
    },
    methods: {
        startQuiz() {
            this.isQuizStarted = true
            this.answers = []

            this.operandLeft = parseInt(Math.random() * 10)
            switch (this.operator) {
                case '-': 
                    this.operandRight = parseInt(Math.random() * (this.operandLeft + 1))
                    break
                case '/':
                    do {
                        this.operandRight = parseInt(Math.random() * 10) + 1
                    } while (this.operandLeft % this.operandRight !== 0)
                    break
                default:
                    this.operandRight = parseInt(Math.random() * 10)
            }

            const method = {
                '+': (a, b) => a + b,
                '-': (a, b) => a - b,
                '/': (a, b) => a / b,
                '*': (a, b) => a * b
            }

            const methodToUse = method[this.operator]

            const expectedAnswer = methodToUse(this.operandLeft, this.operandRight)
            this.correctAnswer = expectedAnswer

            do {
                let answer = 0
                if (expectedAnswer - 3 < 0) {
                    answer = parseInt(Math.random() * (expectedAnswer + 3 - (expectedAnswer - 3)))
                } else {
                    answer = parseInt(Math.random() * 6) + (expectedAnswer - 3)
                }
                
                if (!this.answers.includes(answer)) {
                    this.answers.push(answer)
                }
            } while (this.answers.length < 4)

            if (!this.answers.includes(expectedAnswer)) {
                this.answers[parseInt(Math.random() * this.answers.length)] = expectedAnswer
            }
        },
        selectAnswer(answerSelected) {
            if (answerSelected !== this.correctAnswer) {
                alert('WRONG ANSWER!!!')
            } else {
                this.startQuiz()
            }
        }
    }
}
</script>

<style>

</style>