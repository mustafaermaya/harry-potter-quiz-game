"<template>
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
          <div>
            <div>
              <div
                class="text-white mb-3"
              >{{ currentQuestion.question.number }} - {{ currentQuestion.question.title }}</div>
              <div class="d-flex">
                <b-button
                  pill
                  variant="secondary mr-2 mt-3 mb-3"
                  v-for="(currentOption , index) in currentQuestion.options"
                  :key="index"
                  style="flex-grow:1;"
                  @click="answerCount(currentOption)"
                  :value="currentOption.value"
                  :class="[{'btn btn-success': currentQuestion.question.answered && currentOption.istrue}, {'btn btn-danger': currentQuestion.question.answered && !currentOption.istrue}]"
                  :disabled="currentQuestion.question.answered"
                >{{ currentOption.text }}</b-button>
              </div>
            </div>
            <div>
              <div>
                <button
                  class="btn btn-info mt-3 ml-3"
                  @click="questionPassed"
                  v-show="questionNumber<11"
                  :disabled="currentQuestion.question.answered"
                >Pass</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="container d-flex h-100 align-items-center" v-if="gameEnded">
        <div class="mx-auto text-center">
          <div>
            <div>
              <div class="card bg-success">
                <div class="card-body text-center">
                  <p class="card-text">Your correct answers {{ trueAnswers}}</p>
                </div>
              </div>
              <div class="card bg-warning">
                <div class="card-body text-center">
                  <p class="card-text">Your wrong answers {{ wrongAnswers }}</p>
                </div>
              </div>
              <div class="card bg-info">
                <div class="card-body text-center">
                  <p class="card-text">Your pass answers {{ passCount }}</p>
                </div>
              </div>
              <button class="btn btn-info mt-3" @click="startAgain">Start Again</button>
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
              text: "100 Points",
              value: "100",
              istrue: false,
              selected: false
            },
            {
              text: "150 Points",
              value: "150",
              istrue: true,
              selected: false
            },
            {
              text: "200 Points",
              value: "200",
              istrue: false,
              selected: false
            },
            {
              text: "250 Points",
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
      passCount: 0,
      findTrueOp: []
    };
  },
  methods: {
    startGame() {
      this.gameStarted = true;
      this.findUnAnsweredQuestion();
    },
    startAgain() {
      this.questionNumber = 1;
      this.trueAnswers = 0;
      this.wrongAnswers = 0;
      this.passCount = 0;
      for (let index = 0; index < this.questions.length; index++) {
        const element = (this.questions[index].answered = false);
      }
      this.gameStarted = false;
      this.gameEnded = false;
    },
    timeOut() {
      this.currentQuestion.question.answered = true;
      setTimeout(() => {
        this.questionNumber++;
        this.findUnAnsweredQuestion();
      }, 3000);
    },
    answerCount(currentOption) {
      if (currentOption.istrue) {
        this.trueAnswers++;
      } else {
        this.wrongAnswers++;
      }
      this.timeOut();
    },
    questionPassed() {
      this.passCount++;
      this.timeOut();
    },
    findUnAnsweredQuestion() {
      this.currentQuestion.options = [];
      if (this.questionNumber <= 10) {
        let UnAnsweredQuestion = this.questions.find(
          question => question.number == this.questionNumber
        );
        for (let index in UnAnsweredQuestion.options) {
          let randomIndex = Math.floor(
            Math.random() * UnAnsweredQuestion.options.length
          );
          while (
            this.currentQuestion.options.includes(
              UnAnsweredQuestion.options[randomIndex]
            )
          ) {
            randomIndex = Math.floor(
              Math.random() * UnAnsweredQuestion.options.length
            );
          }
          this.currentQuestion.options[index] =
            UnAnsweredQuestion.options[randomIndex];
        }
        this.currentQuestion.question = UnAnsweredQuestion;
      } else {
        this.gameEnded = true;
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
    url("../img/hogwarts.jpg");
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

"