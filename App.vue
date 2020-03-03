<template>
  <div id="app">
    <meta name="viewport" content="width=device-width, user-scalable=no">
    <link href="https://fonts.googleapis.com/css?family=Roboto&display=swap" rel="stylesheet">
    <div class="bar">
      <div class="bar-content" :style="`width: ${(userResponses.length / questions.length) * 100}%`"></div>
    </div>
    <div v-if="question">
      <div class="question">{{ question.text }}</div>
      <div class="responses">
        <div
          class="response"
          v-for="(value, index) in responses"
          v-on:click="response = index"
          :class="{'active': response === index}">
          <div class="response-image">
            <img :src="value.icon"/>
          </div>
          <div class="response-text">{{ value.text }}</div>
        </div>
      </div>
      <div
        v-if="success === true"
        class="validate success">
        Bonne réponse
      </div>
      <div
        v-else-if="success === false"
        class="validate error">
        Mauvaise réponse
      </div>
      <button
        v-else
        :disabled="response === null"
        v-on:click="validate"
        class="validate">
        Vérifier
      </button>
    </div>
    <div v-else>
      <p class="message-finish">
          Félicitations !<br>
          Vous avez terminé
      </p>
      <button class="restart" v-on:click="init">Recommencer</button>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      questions: [
        { text: "Lequel est un garçon ?", response: 0 },
        { text: "Lequel est une fille ?", response: 1 },
        { text: "Lequel est une femme ?", response: 2 },
        { text: "Lequel est un homme ?", response: 3 },
      ],
      responses: [
        { text: "Boy", icon: require('./images/boy.svg') },
        { text: "Girl", icon: require('./images/girl.svg') },
        { text: "Woman", icon: require('./images/woman.svg') },
        { text: "Man", icon: require('./images/man.svg') },
      ],
      userResponses: [],
      response: null,
      success: null,
    }
  },
  computed: {
    question () {
      return this.questions[this.userResponses.length]
    }
  },
  methods: {
    validate () {
      this.success = this.response === this.question.response
      setTimeout(() => {
        if (this.success) {
          this.userResponses.push(true)
          this.response = null
        }
        this.success = null
      }, 1000)
    },
    init () {
      this.userResponses = []
      this.response = null
    }
  },
  created () {
    this.init()
  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

#app {
  height: 100vh;
  font-family: Roboto;
}
.bar {
  height: 1rem;
  border-radius: 0.4rem;
  background-color: #E0E0E0;
  width: 100%;
  display: flex;
  margin-bottom: 1rem;
}
.bar-content {
  height: 1rem;
  background-color: #4CAF50;
  border-radius: 0.4rem;
}

.question {
  font-weight: 600;
  font-size: 1.5rem;
}
.responses {
  display: flex;
  flex-flow: wrap;
  margin-top: 1rem;
}
.response {
  flex: 0 0 40%;
  box-shadow: 0px 0px 5px #607D8B;
  border-radius: 1rem;
  margin: 2%;
  padding: 3%;
  text-align: center;
}
.response.active {
  background-color: #4CAF50;
}
.response-text {
  margin-top: 0.8rem;
}
.validate {
  width: 100%;
  border: none;
  background-color: #4CAF50;
  color: #fff;
  text-transform: uppercase;
  padding: 0.8rem 0;
  border-radius: 0.4rem;
  font-size: 1.2rem;
  font-weight: 600;
  margin-top: 0.8rem;
  text-align: center;
}
.validate.success {
  background-color: #1B5E20;
}
.validate.error {
  background-color: #F44336;
}

.validate:disabled {
  background-color: #9E9E9E;
}

.restart {
  border: navajowhite;
  padding: 0.8rem 0;
  background-color: #03A9F4;
  color: #fff;
  text-transform: uppercase;
  font-weight: 600;
  width: 100%;
  border-radius: 0.4rem;
  font-size: 1.2rem;
}

.message-finish {
  text-align: center;
  font-size: 1.4rem;
  font-weight: 500;
}
</style>
