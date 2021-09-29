<template>
    <div>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show" class="m-5 align-self-end">
            <div v-for="(item, questionIndex) in form" :key="questionIndex" class="mx-5">
                <div class="d-flex justify-content-end">
                    <b-button variant="outline-secondary" @click="validateOnSubmit && deleteQuestion(questionIndex)" class="border-0"> X </b-button>
                </div>
                <h5 class="text-secondary">Assignment #{{ questionIndex + 1 }}</h5>

                <b-form-group
                    label="Enter the question:"
                    :label-for="'q[' + questionIndex + ']'"
                    description="Try to make it a closed question."
                    :invalid-feedback="invalidQuestionFeedback"
                    label-size="lg"
                >
                    <b-form-input
                        v-model="item.content"
                        type="text"
                        placeholder="Enter question"
                        :state="validateOnSubmit && isQuestionNotEmpty(questionIndex)"
                        :name="'q[' + questionIndex + ']'"
                        required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    :label-for="'ch[' + questionIndex + ']'"
                    :invalid-feedback="invalidObligatoryQuestionFeedback"
                    :state="validateOnSubmit && hasObligatoryQuestion()"
                >
                    <b-form-checkbox
                        v-model="item.isObligatory"
                        :name="'ch[' + questionIndex + ']'"
                        value.boolean="true"
                        unchecked-value.boolean="false"
                    >
                        &nbsp; Obligatory
                    </b-form-checkbox>
                </b-form-group>

                <div v-for="(answer, index) in item.answers" :key="index" class="my-2 ms-5">
                    <b-form-group
                        label="Enter answer:"
                        :label-for="'a[' + questionIndex + '][' + index + ']'"
                        :invalid-feedback="invalidAnswersFeedback"
                    >
                        <b-form-input
                            :value="answer.value"
                            v-model="answer.value"
                            placeholder="Enter answer"
                            :name="'a[' + questionIndex + '][' + index + ']'"
                            @change="onChangeInput(questionIndex)"
                            :state="validateOnSubmit && hasEnoughAnswers(questionIndex)"
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group
                        :label-for="'ch[' + questionIndex + '][' + index + ']'"
                        :invalid-feedback="invalidCorrectAnswerFeedback"
                        :state="validateOnSubmit && hasCorrectAnswer(questionIndex)"
                    >
                        <b-form-checkbox
                            v-model="answer.isCorrect"
                            :name="'ch[' + questionIndex + '][' + index + ']'"
                            value.boolean="true"
                            unchecked-value.boolean="false"
                        >
                            &nbsp; Correct answer
                        </b-form-checkbox>
                    </b-form-group>
                </div>
                <hr class="my-5" />
            </div>

            <div class="mx-5 gx-3">
                <b-button @click="addQuestion" variant="primary"> Add question </b-button>
                <b-button @click="validateForm" type="submit" variant="primary">Submit</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </div>
        </b-form>
        <b-card class="mt-3" header="Form Data Result">
            <pre class="m-0">{{ form }}</pre>
        </b-card>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: [
                {
                    content: "",
                    isObligatory: false,
                    answers: [{ value: "", isCorrect: false }]
                }
            ],
            show: true,
            validateOnSubmit: null,
            invalidQuestionFeedback: "Question is not correct: enter content of question",
            invalidAnswersFeedback: "Not enough answers",
            invalidCorrectAnswerFeedback: "At least one answer must be correct",
            invalidObligatoryQuestionFeedback: "At least one question must be obligatory"
        }
    },
    methods: {
        validateForm() {
            this.validateOnSubmit = true
        },
        onSubmit(event) {
            event.preventDefault()
            alert(JSON.stringify(this.form))
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form = [
                {
                    content: "",
                    isObligatory: false,
                    answers: [{ value: "", isCorrect: false }]
                }
            ]
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
                this.validateOnSubmit = null
            })
        },
        onChangeInput(index) {
            this.form[index].answers = this.form[index].answers.filter((element) => element.value !== "")
            this.form[index].answers.push({ value: "", isCorrect: false })
        },
        addQuestion() {
            this.form.push({
                content: "",
                isObligatory: false,
                answers: [{ value: "", isCorrect: false }]
            })
        },
        deleteQuestion(questionIndex) {
            this.form = this.form.filter((item, index) => index !== questionIndex)
        },
        isQuestionNotEmpty(questionIndex) {
            return this.form[questionIndex].content !== ""
        },
        hasCorrectAnswer(questionIndex) {
            return this.form[questionIndex].answers.reduce((acc, item) => {
                if (item.isCorrect) {
                    acc = true
                }
                return acc
            }, false)
        },
        hasEnoughAnswers(questionIndex) {
            return this.form[questionIndex].answers.length >= 3
        },
        hasObligatoryQuestion() {
            return this.form.reduce((acc, item) => {
                if (item.isObligatory) {
                    acc = true
                }
                return acc
            }, false)
        }
    }
}
</script>
<style scoped>
button {
    margin-right: 1em;
}
</style>