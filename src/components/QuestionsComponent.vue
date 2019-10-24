<template>
  <div>
    <div class="card m-4">
      <div class="card-body" v-if="!gameStarted">
        <h3>Hello! Welcome to the Harry Potter Quiz Game</h3>
        <p>It's about to start</p>
        <p>Click and play</p>
        <button id="start_game" class="btn btn-success" @click="startGame">Start Game</button>
      </div>
      <div class="card m-4" v-if="gameStarted && !gameEnded">
        <div class="card-body">
          <div>{{ currentQuestion.question.number }} - {{ currentQuestion.question.title }}</div>
          <div class="d-flex">
            <button
              class="btn btn-secondary ml-1 mt-3"
              v-for="(currentOption , index) in currentQuestion.options"
              :key="index"
              style="flex-grow:1;"
              @click="disable(currentOption)"
              :value="currentOption.value"
              :class="[{'btn btn-success': currentQuestion.question.answered && currentOption.istrue}, {'btn btn-danger': currentQuestion.question.answered && !currentOption.istrue}]"
              :disabled="currentQuestion.question.answered"
            >{{ currentOption.text }}</button>
          </div>
          <p
            class="mt-5"
            v-if="currentQuestion.question.answered"
          >The correct answer is {{ trueOption }}</p>
        </div>
        <div class="card m-4">
          <div class="card-body">
            <button
              class="btn btn-primary"
              @click="previousQuestion"
              v-if="1<questionNumber"
            >Previous Question</button>
            <button
              class="btn btn-primary ml-3"
              @click="nextQuestion"
              v-show="questionNumber<10"
            >Next Question</button>
            <button
              class="btn btn-info ml-3"
              v-show="questionNumber==10"
              v-if="!gameEnded"
              @click="endGame"
            >Show The Results</button>
          </div>
        </div>
      </div>
      <div class="card m-4" v-if="gameEnded">
        <div class="card-body">
          <p>Your correct answers {{ trueAnswers }}</p>
          <p>Your wrong answers {{ wrongAnswers }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          number: 1,
          title: "What is Lord Voldemort’s real name?",
          answered: false,
          options: [
            {
              text: "Tom Marvolo Riddle",
              value: "Marvolo",
              istrue: true,
              selected: false
            },
            {
              text: "Tom Marvilo Riddle",
              value: "Marvilo",
              istrue: false,
              selected: false
            },
            {
              text: "Tom Marvin Riddle",
              value: "Marvin",
              istrue: false,
              selected: false
            },
            {
              text: "Tom Ravolo Riddle",
              value: "Ravolo",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 2,
          title:
            "What’s the LAST line of the book, Harry Potter and the Sorcerer’s Stone?",
          answered: false,
          options: [
            {
              text: "“Goodbye, for now, Hogwarts.”",
              value: "Hogwarts",
              istrue: false,
              selected: false
            },
            {
              text:
                "“Harry, Ron, and Hermione looked at each other sheepishly and smiled.”",
              value: "Harry",
              istrue: false,
              selected: false
            },
            {
              text: "“I’m going to have a lot of fun with Dudley this summer…”",
              value: "Dudley",
              istrue: true,
              selected: false
            },
            {
              text: "“’Until the fall,’ said Dumbledore with a nod.”",
              value: "Dumbledore",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 3,
          title: "According to the Dursleys, how did Harry’s parents die?",
          answered: false,
          options: [
            {
              text: "In a plane crash",
              value: "plane",
              istrue: false,
              selected: false
            },
            {
              text: "In a train crash",
              value: "train",
              istrue: false,
              selected: false
            },
            {
              text: "In a bus crash",
              value: "bus",
              istrue: false,
              selected: false
            },
            {
              text: "In a car crash",
              value: "car",
              istrue: true,
              selected: false
            }
          ]
        },
        {
          number: 4,
          title: "How many points is the snitch worth in Quidditch?",
          answered: false,
          options: [
            {
              text: "100",
              value: "100",
              istrue: false,
              selected: false
            },
            {
              text: "150",
              value: "150",
              istrue: true,
              selected: false
            },
            {
              text: "200",
              value: "200",
              istrue: false,
              selected: false
            },
            {
              text: "250",
              value: "250",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 5,
          title: "Who put Harry’s name in the Goblet of Fire?",
          answered: false,
          options: [
            {
              text: "Peter Pettigrew",
              value: "peter",
              istrue: false,
              selected: false
            },
            {
              text: "Severus Snape",
              value: "severus",
              istrue: false,
              selected: false
            },
            {
              text: "Cornelius Fudge",
              value: "cornelius",
              istrue: false,
              selected: false
            },
            {
              text: "Barty Crouch, Jr.",
              value: "barty",
              istrue: true,
              selected: false
            }
          ]
        },
        {
          number: 6,
          title:
            "What does one say to close the Marauder's Map and make it blank again?",
          answered: false,
          options: [
            {
              text: "Nothing to See Here",
              value: "nothing to see here",
              istrue: false,
              selected: false
            },
            {
              text: "All Done",
              value: "all done",
              istrue: false,
              selected: false
            },
            {
              text: "Mischief Managed",
              value: "mischief managed",
              istrue: true,
              selected: false
            },
            {
              text: "Hello Professor",
              value: "hello professor",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 7,
          title:
            "Who has been stealing Harry's letters from Ron and Hermione at the beginning of 'Harry Potter and the Chamber of Secrets'?",
          answered: false,
          options: [
            {
              text: "Dumbledore",
              value: "dumbledore",
              istrue: false,
              selected: false
            },
            {
              text: "Draco Malfoy",
              value: "draco malfoy",
              istrue: false,
              selected: false
            },
            {
              text: "Dobby",
              value: "dobby",
              istrue: true,
              selected: false
            },
            {
              text: "The Dursleys",
              value: "dursleys",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 8,
          title: "What's the name of Filch's cat?",
          answered: false,
          options: [
            {
              text: "Mrs. Norris",
              value: "mrs. norris",
              istrue: true,
              selected: false
            },
            {
              text: "Butter Cup",
              value: "butter cup",
              istrue: false,
              selected: false
            },
            {
              text: "Jones",
              value: "jones",
              istrue: false,
              selected: false
            },
            {
              text: "Ser Pounce",
              value: "ser pounce",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 9,
          title: "What is the model of the first broom Harry ever receives?",
          answered: false,
          options: [
            {
              text: "Firebolt",
              value: "firebolt",
              istrue: false,
              selected: false
            },
            {
              text: "Nimbus 2000",
              value: "nimbus 2000",
              istrue: true,
              selected: false
            },
            {
              text: "Cleansweep One",
              value: "cleansweep one",
              istrue: false,
              selected: false
            },
            {
              text: "Hoover",
              value: "hoover",
              istrue: false,
              selected: false
            }
          ]
        },
        {
          number: 10,
          title:
            "Who comes to pick up Harry when he's old enough to go to Hogwarts?",
          answered: false,
          options: [
            {
              text: "Nymphadora Tonks",
              value: "nymphadora tonks",
              istrue: false,
              selected: false
            },
            {
              text: "Hagrid",
              value: "hagrid",
              istrue: true,
              selected: false
            },
            {
              text: "Mrs. Weasley",
              value: "mrs. weasley",
              istrue: false,
              selected: false
            },
            {
              text: "Dumbledore",
              value: "dumbledore",
              istrue: false,
              selected: false
            }
          ]
        }
      ],
      gameStarted: false,
      gameEnded: false,
      questionNumber: 1,
      currentQuestion: [
        {
          question: "",
          options: []
        }
      ],
      trueAnswers: 0,
      wrongAnswers: 0,
      findTrueOp: [],
      trueOption: "",
      countDown: 30
    };
  },
  methods: {
    startGame() {
      this.gameStarted = true;
      this.findUnansweredQuestion();
    },
    endGame() {
      if (!this.currentQuestion.question.answered) {
        this.$swal("Hello Vue world!!!");
      } else {
        this.gameEnded = true;
      }
    },
    disable(currentOption) {
      this.currentQuestion.question.options.map(option => {
        this.findTrueOp.push(option);
      });
      let trues = this.findTrueOp.find(trueOp => trueOp.istrue);
      this.trueOption = trues.text;
      if (currentOption.istrue) {
        this.trueAnswers++;
      } else {
        this.wrongAnswers++;
      }
      this.findTrueOp = [];
      this.currentQuestion.question.answered = true;
    },
    findUnansweredQuestion() {
      this.currentQuestion.options = [];
      let UnAnsweredQuestion = this.questions.find(
        question => question.number == this.questionNumber
      );
      UnAnsweredQuestion.options.map(option => {
        this.currentQuestion.options.push(option);
      });
      this.currentQuestion.question = UnAnsweredQuestion;
    },
    nextQuestion() {
      if (!this.currentQuestion.question.answered) {
        this.$swal("You didn't answer the question");
      } else {
        this.questionNumber++;
        this.findUnansweredQuestion();
      }
    },
    previousQuestion() {
      this.questionNumber--;
      this.findUnansweredQuestion();
    }
  }
};
</script>

<style scoped>
</style>

