<template>
  <div>
    <Header :numTotal="numTotal" :numCorrect="numCorrect" />
    <b-container>
      <b-row>
        <b-col class="mx-auto" sm="12" md="6" v-if="this.index <= 9">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
        <b-col class="mx-auto" sm="12" md="6" v-else>
          <b-card
            bg-variant="info"
            text-variant="white"
            header="Thanks for taking the quiz"
            class="text-center"
          >
            <b-card-text>
              Correct Answer : {{this.numCorrect}} / {{this.numTotal}}
            </b-card-text>
            <b-card-text>
              Your Point : {{point}}
            </b-card-text>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header';
import QuestionBox from './components/QuestionBox';
export default {
  data() {
    return {
      questions: [],
      index: 0,
      loading: true,
      errored: false,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  components: {
    Header,
    QuestionBox,
  },
  mounted: function() {
    fetch(`https://opentdb.com/api.php?amount=10&category=27&type=multiple`)
      .then((res) => {
        return res.json();
      })
      .then((data) => {
        this.questions = data.results;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => {
        this.loading = false;
      });
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  computed: {
    point() {
      let point = (this.numCorrect / this.numTotal) * 100
      return point
    }
  }
};
</script>

<style></style>
