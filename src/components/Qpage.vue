<template>
<form @submit.prevent="validateBeforeSubmit">
<div class="columns" v-for="question in questions" :key='question.id'>

        <div class="column is-one-third">
            <p> {{question.question}} </p>         
        </div>
        <div class="column">
            </div>

        <div class="control column is-two-thirds">
            <label class="radio">
            <input type="radio" :name="question.questionID" @blur="currentAnswer(question.questionID, 1)" v-validate="'required'" value="1">
            Disagree
            </label>
            <label class="radio">
                <input type="radio" :name="question.questionID"  @blur="currentAnswer(question.questionID, 2)" v-validate="'required'" value="2">
                Slightly Disagree
            </label>
            <label class="radio">
                <input type="radio" :name="question.questionID"  @blur="currentAnswer(question.questionID, 3)" v-validate="'required'" value="3">
                Slightly Agree
            </label>
            <label class="radio">
                <input type="radio" :name="question.questionID"    @blur="currentAnswer(question.questionID, 4)" v-validate="'required'" value="4">
               Agree
            </label>
            <p class="has-text-danger	" v-show="errors.has(question.questionID)">Please select an answer</p>
        </div>    

    </div>
    <button type="submit" :disabled="errors.any()" class="button" placeholder="next"> <span v-if="qCount < 4">Next</span> <span v-if="qCount===4">Submit</span> </button>
    </form>
</template>

<script>
export default {
  name: "Qpage",
  data: function() {
    return {};
  },
  props: {
    questions: Array,
    qCount: Number
  },
  methods: {
    qNext() {},
    validateBeforeSubmit() {
      this.$validator.validateAll().then(result => {
        if (result) {
          if (this.qCount === 4) {
            this.$emit("calculate");
          }
          this.$emit("nextQ");
          return;
        }

        alert("Correct them errors!");
      });
    },
    currentAnswer(name, value) {
      this.$emit("sentAnswer", {
        ID: name,
        answer: value
      });
    }
  }
};
</script>

<style>
.radio {
  margin: auto 30px;
}
</style>


