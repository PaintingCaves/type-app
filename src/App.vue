<template>
  <div id="app" class="container" >

    <welcome @startClick="saveUser" :qCount="qCount" v-show="qCount == 0"></welcome>
    <qpage v-show="qCount ==1" :qCount="qCount" @sentAnswer="saveAnswer" @nextQ="nextPage" :questions="firstPage"></qpage>
    <qpage v-show="qCount ==2" :qCount="qCount" @sentAnswer="saveAnswer" @nextQ="nextPage" :questions="secondPage"></qpage>
    <qpage v-show="qCount ==3" :qCount="qCount" @sentAnswer="saveAnswer" @nextQ="nextPage" :questions="thirdPage"></qpage>
    <qpage v-show="qCount ==4" :qCount="qCount" @sentAnswer="saveAnswer" @nextQ="nextPage" @calculate="calculateResult" :questions="fourthPage"></qpage>
    <results v-show="qCount ==5"></results>
    <button class="button" @click="prevPage" v-if="qCount >0">Prev</button>
    <!-- <button class="button" @click="nextPage" v-if="qCount <6">Next</button> -->

  </div>


</template>

<script>
import Qpage from "./components/Qpage.vue";
import Welcome from "./components/Welcome.vue";
import Results from "./components/Results.vue";
import json from "./assets/questions.json";

export default {
  name: "app",
  methods: {
    nextPage() {
      this.qCount++;
    },
    calculateResult() {
      // let i;
      // for (i = 0; i < 10; i++) {
      //   this.EQ += this.responses[i].answer;
      // }
      // for (i = 10; i < 20; i++) {
      //   this.SQ += this.responses[i].answer;
      // }
      // for (i = 20; i < 30; i++) {
      //   this.EX += this.responses[i].answer;
      // }
      // for (i = 30; i < 40; i++) {
      //   this.AR += this.responses[i].answer;
      // }
      if (
        this.SQ > 15 &&
        this.EQ > 32 &&
        this.SQ - this.EQ < 8 &&
        this.EX > 20 &&
        this.AR > 28
      ) {
        this.devType = "FED";
      } else if (
        this.SQ > 15 &&
        this.EQ > 20 &&
        this.SQ - this.EQ < 8 &&
        this.EX > 20 &&
        this.AR > 25
      ) {
        this.devType = "FSFED";
      } else if (this.SQ > 28 && this.SQ - this.EQ > 5 && this.EX < 25) {
        this.devType = "FSBED";
      } else if (this.SQ > 33 && this.SQ - this.EQ > 7 && this.EX < 20) {
        this.devType = "BED";
      }
      if (this.EQ < 22 && this.EX < 22) {
        this.workType = "Alone";
      } else if (this.EQ > 28 && this.EX > 28) {
        this.workType = "People";
      } else if (this.EX < 20) {
        this.workType = "Mixed-alone";
      } else if (this.ex > 21) {
        this.workType = "Mixed-people";
      }
    },
    prevPage() {
      this.qCount--;
    },
    saveUser(event) {
      this.firstName = event.firstName;
      this.lastName = event.lastName;
      this.email = event.email;
      this.nextPage();
    },
    saveAnswer(event) {
      this.responses[event.ID - 1] = {
        id: event.ID,
        answer: event.answer
      };
    }
  },

  components: {
    Qpage,
    Welcome,
    Results
  },
  data: function() {
    return {
      questions: json,
      qCount: 0,
      firstName: "",
      lastName: "",
      email: "",
      test: "0",
      firstPage: [],
      secondPage: [],
      thirdPage: [],
      fourthPage: [],
      responses: [],
      SQ: 0,
      EQ: 0,
      EX: 0,
      AR: 0,
      devType: "",
      workType: ""
    };
  },
  mounted: function() {
    this.firstPage = this.questions.filter(obj => {
      return obj.questionID % 4 === 1;
    });
    this.secondPage = this.questions.filter(obj => {
      return obj.questionID % 4 === 2;
    });
    this.thirdPage = this.questions.filter(obj => {
      return obj.questionID % 4 === 3;
    });
    this.fourthPage = this.questions.filter(obj => {
      return obj.questionID % 4 === 0;
    });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
