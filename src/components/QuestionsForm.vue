<template>
    <div>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
            <div v-for="(item, qindex) in form" :key="qindex" class="m-5">
                <b-form-group
                    label="Enter the question:"
                    label-for="input-1"
                    description="Try to make it a closed question."
                >
                    <b-form-input
                        v-model="item.question"
                        type="text"
                        placeholder="Enter question"
                        required
                    ></b-form-input>
                </b-form-group>

                <div
                    v-for="(answer, index) in item.answers"
                    :key="index"
                    class="my-2"
                >
                    <b-form-group label="Enter answer:" label-for="input-2">
                        <b-form-input
                            :value="answer.value"
                            v-model="answer.value"
                            placeholder="Enter answer"
                            @change="onChangeInput(qindex)"
                        ></b-form-input>

                        <b-form-checkbox
                            v-model="answer.isCorrect"
                            name="checkbox-1"
                            value="correct"
                            unchecked-value="correct"
                        >
                            Correct answer
                        </b-form-checkbox>
                    </b-form-group>
                </div>
                <hr class="my-5" />
            </div>

            <div class="mx-5 gx-3">
                <b-button @click="addQuestion" variant="primary">
                    Add question
                </b-button>
                <b-button type="submit" variant="primary">Submit</b-button>
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
                    question: "",
                    answers: [{ value: "", isCorrect: false }]
                }
            ],
            show: true
        }
    },
    methods: {
        onSubmit(event) {
            event.preventDefault()
            alert(JSON.stringify(this.form))
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form = [
                {
                    question: "",
                    answers: [{ value: "", isCorrect: false }],
                    checked: []
                }
            ]
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        },
        onChangeInput(index) {
            this.form[index].answers = this.form[index].answers.filter(
                (element) => element.value !== ""
            )
            this.form[index].answers.push({ value: "", isCorrect: false })
        },
        addQuestion() {
            this.form.push({
                question: "",
                answers: [{ value: "", isCorrect: false }]
            })
        }
    }
}
</script>
<style>
button {
    margin-right: 1em;
}
</style>