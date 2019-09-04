<template>
  <div class="pt-5">
    <b-container>
      <b-row class="mb-5">
        <b-col><h6>So far, you got {{correct}} out of {{total - 1}} correct.</h6></b-col>
      </b-row>
      <b-row class="mb-5">
        <b-col><h1>{{a}} x {{b}}</h1></b-col>
      </b-row>
      <div class="answers mb-5">
        <b-row>
          <b-col v-for="num in answers" :key="num">
            <b-button :disabled="freeze" v-on:click="checkAnswer(num)" variant="primary">{{num}}</b-button>
          </b-col>
        </b-row>
      </div>
      <div v-if="freeze">
        <h2 class="correct" v-if="showCorrect">Correct!</h2>
        <h2 class="wrong" v-if="showWrong">Wrong, {{a}} x {{b}} is {{answer}}</h2>
      </div>
    </b-container>
  </div>
</template>

<script>
var shuffle = function (array) {

	var currentIndex = array.length;
	var temporaryValue, randomIndex;

	// While there remain elements to shuffle...
	while (0 !== currentIndex) {
		// Pick a remaining element...
		randomIndex = Math.floor(Math.random() * currentIndex);
		currentIndex -= 1;

		// And swap it with the current element.
		temporaryValue = array[currentIndex];
		array[currentIndex] = array[randomIndex];
		array[randomIndex] = temporaryValue;
	}

	return array;

};

export default {
  name: 'HelloWorld',
  data: () => ({
    low: 1,
    high: 12,
    a: 0,
    b: 0,
    correct: 0,
    total: 0,
    answer: 1,
    freeze: false,
    showCorrect: false,
    showWrong: false,
    answers: [],
  }),
  mounted: function () {
    this.generateNewNumber();
  },
  props: {
    msg: String
  },
  methods: {
    generateNewNumber: function() {
      this.total++;
      this.freeze = false;
      this.showCorrect = false;
      this.showWrong = false;
      this.a = parseInt(Math.random() * this.high) + this.low;
      this.b = parseInt(Math.random() * this.high) + this.low;
      this.answer = this.a * this.b;
      const allPossibleAnswers = new Array(12).fill(1).map((v, i) => { 
        return (i + 1) * this.a;
      }).filter(a => a !== this.answer)
      shuffle(allPossibleAnswers);
      this.answers = [this.answer, ...allPossibleAnswers.slice(0, 4)]; 
      this.answers.sort((a, b) => a - b);
    },
    checkAnswer: function (n) {
      if (n === this.answer) {
        this.showCorrect = true;
      } else {
        this.showWrong = true;
      }
      this.freeze = true;
      setTimeout(() => {
        if (n === this.answer) {
        this.correct++;
      }
        this.generateNewNumber();
      }, 1500);
    }
  }
}
</script>

<style scoped>
.answers {
  width: 500px;
  margin: 0 auto;
}

button {
  width: 60px;
}

.wrong {
  color: red;
}

.correct {
  color: green;
}
</style>
