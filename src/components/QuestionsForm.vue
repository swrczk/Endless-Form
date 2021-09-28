<template>
    <div>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
            <b-form-group
                id="input-group-1"
                label="Enter the question:"
                label-for="input-1"
                description="Try to make it a closed question."
            >
                <b-form-input
                    id="input-1"
                    v-model="form.question"
                    type="text"
                    placeholder="Enter question"
                    required
                ></b-form-input>
            </b-form-group>

            <div v-for="(answer, index) in form.answers" :key="index">
                <b-form-group
                    id="input-group-2"
                    label="Enter answer:"
                    label-for="input-2"
                >
                    <b-form-input
                        id="input-2"
                        v-model="answer.value"
                        placeholder="Enter answer"
                        @change="onChangeInput"
                    ></b-form-input>

                    <b-form-checkbox
                        id="checkbox-1"
                        v-model="answer.isCorrect"
                        name="checkbox-1"
                        value="correct"
                        unchecked-value="correct"
                    >
                        Correct answer
                    </b-form-checkbox>
                </b-form-group>
            </div>

            <b-button @click="addQuestion" variant="primary">
                Add question
            </b-button>
            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button type="reset" variant="danger">Reset</b-button>
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
            form: {
                question: "",
                answers: [{ value: "", isCorrect: false }]
            },
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
            this.form = {
                question: "",
                answers: [{ value: "", isCorrect: false }],
                checked: []
            }
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        },
        onChangeInput() {
            this.form.answers = this.form.answers.filter(
                (element) => element.value !== ""
            )
            this.form.answers.push({ value: "", isCorrect: false })
        }
    }
}
</script>