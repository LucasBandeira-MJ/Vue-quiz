<template>
  <div id="app">

    <div class="quiz">
      <p class="title">O ano de 2019 chegou ao fim e a tecnologia fez parte do nosso dia a dia! Pensando nisso, fizemos um quiz para testar seus conhecimentos e descobrir se você esteve por dentro das matérias do Dialogando este ano.</p>


      <div class="question" v-for="question in questions" :key="question.id"
      >
        <p>{{question.id + 1}} . {{question.question}}</p>

        <button @click="Check(question.truefalse, 0)"
        class="verdade" :disabled="question.id != 0">
          VERDADEIRO
        </button>

        <button @click="Check(!question.truefalse, 1)"
        class="falso" :disabled="question.id != 0">

          FALSO
        </button>


        <div class="answer"
          :class="{ visible: acertoerro[question.id]}">

          <img src="./assets/img/sucesso.svg" alt="resposta" />
          <p class="response_message">{{acertoerro[question.id]}}</p>

          <p class="response_message--sub">{{question.noticia}}</p>

          <p class="response_answer">{{question.answer}}</p>
        </div>
      </div>
      


    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      questions: [],
      acertoerro: [],
      gotright: [],
      disabled: [],
      quests: [],
      verdades: [],
      falsos: [],
      counter: 0,
      countpoints: 0
    };
  },
  async created() {
    axios
      .get("./questions.json")
      .then(res => (this.questions = res.data))
      .catch(err => console.log(err));

      this.quests = document.getElementsByClassName("question");
      this.verdades = document.getElementsByClassName("verdade");
      this.falsos = document.getElementsByClassName("falso");
  },
  methods: {

    Check(x, y) {
      if(x) { 
        this.acertoerro.push("você acertou!");
        this.gotright.push(x);
        this.quests[this.counter].classList.add("right")
        }
      else { 
        this.acertoerro.push("você errou!");
        this.gotright.push(x);
        this.quests[this.counter].classList.add("wrong")
      }
      this.verdades.forEach(verdade => { verdade.disabled = true; });
      this.falsos.forEach(falso => { falso.disabled = true; });

      if(this.counter < 6){
        this.verdades[this.counter+1].disabled = false;
        this.falsos[this.counter+1].disabled = false;
      }
      else{
        this.Monolith()
      }

      if(!y){ this.verdades[this.counter].classList.add('glow') } 
      else{ this.falsos[this.counter].classList.add('glow') }

      this.disabled.push(true);
      this.counter ++;
    },
    Monolith() {
      this.acertoerro.forEach(e => {
        if(e == "você acertou!"){ this.countpoints ++ }
      });
    }

  }
};
</script>

<style lang="stylus">
@import './assets/stylus/core';
@import './assets/stylus/styles';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
