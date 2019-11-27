<template>
  <div>
    <header class="masthead">
      <div class="container d-flex h-100 align-items-center" v-if="!gameStarted">
        <div class="mx-auto text-center">
          <h1 class="mx-auto my-0 text-uppercase">Harry Potter</h1>
          <h2 class="text-white-50 mx-auto mt-2 mb-5">
            Welcome to Harry Potter Fan Game.
            <br />
            <br />The game is about to start
          </h2>
          <b-button pill class="btn btn-info js-scroll-trigger" @click="startGame">Get Started</b-button>
        </div>
      </div>
      <div class="container d-flex h-100 align-items-center" v-if="gameStarted && !gameEnded">
        <div class="mx-auto text-center">
          <div class="text-white mb-3">{{questionNumber}} - {{ currentQuestion.title }}</div>
          <div class="d-flex">
            <b-button
              pill
              variant="secondary mr-2 mt-3 mb-3"
              v-for="(currentOption , index) in currentQuestion.options"
              :key="index"
              style="flex-grow:1;"
              :class="[{'btn btn-success': currentQuestion.answered && currentOption.istrue}, {'btn btn-danger': currentQuestion.answered && !currentOption.istrue}]"
              :disabled="currentQuestion.answered"
              @click="answerCount(currentOption)"
            >{{ currentOption.text }}</b-button>
          </div>
          <p class="mt-3 text-white" v-if="currentQuestion.answered">
            The correct answer is :
            <span class="mt-3 text-warning">{{ trueOptionValue }}</span>
          </p>
          <div>
            <button
              class="btn btn-warning mt-3 ml-3"
              style="width:100px;"
              @click="questionPassed"
              v-show="currentQuestionIndex<11"
              :disabled="currentQuestion.answered"
            >Pass</button>
          </div>
          <div>
            <p class="mt-3 text-white">Remaining Time : {{timeCount}}</p>
          </div>
        </div>
      </div>
      <div class="container d-flex h-100 align-items-center" v-if="gameEnded">
        <div class="mx-auto text-center">
          <div>
            <div>
              <div class="card bg-success">
                <div class="card-body text-center">
                  <p class="card-text">Correct answers {{ trueAnswers}}</p>
                </div>
              </div>
              <div class="card bg-warning">
                <div class="card-body text-center">
                  <p class="card-text">Wrong answers {{ wrongAnswers }}</p>
                </div>
              </div>
              <div class="card bg-info">
                <div class="card-body text-center">
                  <p class="card-text">Passed questions {{ passCount }}</p>
                </div>
              </div>
              <button class="btn btn-light mt-3" @click="startAgain">Start Again</button>
            </div>
          </div>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          title: "What is Lord Voldemort’s real name?",
          answered: false,
          options: [
            {
              text: "Tom Marvolo Riddle",
              istrue: true
            },
            {
              text: "Tom Marvilo Riddle",
              istrue: false
            },
            {
              text: "Tom Marvin Riddle",
              istrue: false
            },
            {
              text: "Tom Ravolo Riddle",
              istrue: false
            }
          ]
        },
        {
          title:
            "What’s the LAST line of the book, Harry Potter and the Sorcerer’s Stone?",
          answered: false,
          options: [
            {
              text: "“Goodbye, for now, Hogwarts.”",
              istrue: false
            },
            {
              text:
                "“Harry, Ron, and Hermione looked at each other sheepishly and smiled.”",
              istrue: false
            },
            {
              text: "“I’m going to have a lot of fun with Dudley this summer…”",
              istrue: true
            },
            {
              text: "“’Until the fall,’ said Dumbledore with a nod.”",
              istrue: false
            }
          ]
        },
        {
          title: "According to the Dursleys, how did Harry’s parents die?",
          answered: false,
          options: [
            {
              text: "In a plane crash",
              istrue: false
            },
            {
              text: "In a train crash",
              istrue: false
            },
            {
              text: "In a bus crash",
              istrue: false
            },
            {
              text: "In a car crash",
              istrue: true
            }
          ]
        },
        {
          title: "How many points is the snitch worth in Quidditch?",
          answered: false,
          options: [
            {
              text: "100 Points",
              istrue: false
            },
            {
              text: "150 Points",
              istrue: true
            },
            {
              text: "200 Points",
              istrue: false
            },
            {
              text: "250 Points",
              istrue: false
            }
          ]
        },
        {
          title: "Who put Harry’s name in the Goblet of Fire?",
          answered: false,
          options: [
            {
              text: "Peter Pettigrew",
              istrue: false
            },
            {
              text: "Severus Snape",
              istrue: false
            },
            {
              text: "Cornelius Fudge",
              istrue: false
            },
            {
              text: "Barty Crouch, Jr.",
              istrue: true
            }
          ]
        },
        {
          title:
            "What does one say to close the Marauder's Map and make it blank again?",
          answered: false,
          options: [
            {
              text: "Nothing to See Here",
              istrue: false
            },
            {
              text: "All Done",
              istrue: false
            },
            {
              text: "Mischief Managed",
              istrue: true
            },
            {
              text: "Hello Professor",
              istrue: false
            }
          ]
        },
        {
          title:
            "Who has been stealing Harry's letters from Ron and Hermione at the beginning of 'Harry Potter and the Chamber of Secrets'?",
          answered: false,
          options: [
            {
              text: "Dumbledore",
              istrue: false
            },
            {
              text: "Draco Malfoy",
              istrue: false
            },
            {
              text: "Dobby",
              istrue: true
            },
            {
              text: "The Dursleys",
              istrue: false
            }
          ]
        },
        {
          title: "What's the name of Filch's cat?",
          answered: false,
          options: [
            {
              text: "Mrs. Norris",
              istrue: true
            },
            {
              text: "Butter Cup",
              istrue: false
            },
            {
              text: "Jones",
              istrue: false
            },
            {
              text: "Ser Pounce",
              istrue: false
            }
          ]
        },
        {
          title: "What is the model of the first broom Harry ever receives?",
          answered: false,
          options: [
            {
              text: "Firebolt",
              istrue: false
            },
            {
              text: "Nimbus 2000",
              istrue: true
            },
            {
              text: "Cleansweep One",
              value: "cleansweep one",
              istrue: false
            },
            {
              text: "Hoover",
              istrue: false
            }
          ]
        },
        {
          title:
            "Who comes to pick up Harry when he's old enough to go to Hogwarts?",
          answered: false,
          options: [
            {
              text: "Nymphadora Tonks",
              istrue: false
            },
            {
              text: "Hagrid",
              istrue: true
            },
            {
              text: "Mrs. Weasley",
              istrue: false
            },
            {
              text: "Dumbledore",
              istrue: false
            }
          ]
        }
      ],
      gameStarted: false,
      gameEnded: false,
      questionNumber: 0,
      currentQuestionIndex: 0,
      currentQuestion: {},
      mixedQuestions: [],
      mixedOptions: [],
      trueAnswers: 0,
      wrongAnswers: 0,
      passCount: 0,
      trueOptionValue: "",
      timeCount: 30,
      timeFunc: null
    };
  },
  methods: {
    startGame() {
      this.gameStarted = true;
      this.mixQuestion();
      this.getCurrentQuestion();
    },
    getCurrentQuestion() {
      this.timeCount = 30;
      if (this.currentQuestionIndex < this.questions.length) {
        this.currentQuestion = this.mixedQuestions[this.currentQuestionIndex];
        this.mixOptions();
        this.Interval();
        this.questionNumber++;
        this.currentQuestionIndex++;
        this.currentQuestion.options = this.mixedOptions;
      } else {
        this.gameEnded = true;
      }
    },
    questionPassed() {
      this.passCount++;
      this.timeOut();
      clearInterval(this.timeFunc);
      this.findTrueOp();
    },
    answerCount(currentOption) {
      if (currentOption.istrue) {
        this.trueAnswers++;
      } else {
        this.wrongAnswers++;
      }
      this.currentQuestion.answered = true;
      this.timeOut();
      clearInterval(this.timeFunc);
      this.findTrueOp();
    },
    findTrueOp() {
      let trueOption = this.currentQuestion.options.find(
        optionTrue => optionTrue.istrue == true
      );
      this.trueOptionValue = trueOption.text;
    },
    Interval() {
      let interval = setInterval(() => {
        this.timeCount--;
        if (this.timeCount == 0) {
          clearInterval(interval);
          this.currentQuestion.answered = true;
          this.wrongAnswers++;
          this.timeCount = "Time is over";
          this.timeOut();
          this.findTrueOp();
        }
      }, 1000);
      this.timeFunc = interval;
    },
    timeOut() {
      this.currentQuestion.answered = true;
      setTimeout(() => {
        this.getCurrentQuestion();
      }, 5000);
    },
    startAgain() {
      this.currentQuestionIndex = 0;
      this.trueAnswers = 0;
      this.wrongAnswers = 0;
      this.questionNumber = 0;
      for (let index = 0; index < this.questions.length; index++) {
        this.questions[index].answered = false;
      }
      this.gameStarted = false;
      this.gameEnded = false;
    },
    mixQuestion() {
      this.mixedQuestions = [];
      for (let questionIndex in this.questions) {
        let randomQuestionIndex = Math.floor(
          Math.random() * this.questions.length
        );
        while (
          this.mixedQuestions.includes(this.questions[randomQuestionIndex])
        ) {
          randomQuestionIndex = Math.floor(
            Math.random() * this.questions.length
          );
        }
        this.mixedQuestions[questionIndex] = this.questions[
          randomQuestionIndex
        ];
      }
    },
    mixOptions() {
      this.mixedOptions = [];
      for (let optionIndex in this.mixedQuestions[this.currentQuestionIndex]
        .options) {
        let randomOptionIndex = Math.floor(
          Math.random() *
            this.mixedQuestions[this.currentQuestionIndex].options.length
        );
        while (
          this.mixedOptions.includes(
            this.mixedQuestions[this.currentQuestionIndex].options[
              randomOptionIndex
            ]
          )
        ) {
          randomOptionIndex = Math.floor(
            Math.random() *
              this.mixedQuestions[this.currentQuestionIndex].options.length
          );
        }
        this.mixedOptions[optionIndex] = this.mixedQuestions[
          this.currentQuestionIndex
        ].options[randomOptionIndex];
      }
    }
  }
};
</script>

<style scoped>
.masthead {
  position: relative;
  width: 100%;
  height: auto;
  min-height: 35rem;
  padding: 15rem 0;
  background: linear-gradient(
      to bottom,
      rgba(22, 22, 22, 0.3) 0%,
      rgba(22, 22, 22, 0.7) 75%,
      #161616 100%
    ),
    url("../assets/hogwarts.jpg");
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: scroll;
  background-size: cover;
}

.masthead h1 {
  font-family: "Varela Round";
  font-size: 2.5rem;
  line-height: 2.5rem;
  letter-spacing: 0.8rem;
  background: -webkit-linear-gradient(
    rgba(255, 255, 255, 0.9),
    rgba(255, 255, 255, 0)
  );
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
}

.masthead h2 {
  max-width: 20rem;
  font-size: 1rem;
}

@media (min-width: 768px) {
  .masthead h1 {
    font-size: 4rem;
    line-height: 4rem;
  }
}

@media (min-width: 992px) {
  .masthead {
    height: 100vh;
    padding: 0;
  }
  .masthead h1 {
    font-size: 6.5rem;
    line-height: 6.5rem;
    letter-spacing: 0.8rem;
  }
  .masthead h2 {
    max-width: 30rem;
    font-size: 1.25rem;
  }
}
</style>
