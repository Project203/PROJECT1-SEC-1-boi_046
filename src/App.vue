<script setup>
import data from "./assets/data-mockup.json";
import { ref, reactive } from "vue";

let user = ref({name:"Jolly Dev",ages:23})
let now = ref({})
let endGame = ref()
const score = reactive({ AScore: 0, BScore: 0 })
const getNewQuestion = (no) => {
  const newQuest = data.find(e => e.No == no)
  console.log(newQuest)
  if (newQuest === undefined) {
    endGame.value = "THE END"
    now.value = {}
  } else {
    now.value = newQuest
    return data.find(e => e.No == no)
  }

}

const gameStart = () => {
  now.value = getNewQuestion(1)
}

const select = (event) => {
  let selected = getCurrentQuestion()?.Option.find(e => e.oid === event.target.id)
  console.log(selected.score)
  score.AScore += selected.score.AScore
  score.BScore = Number(score.BScore) + Number(selected.score.BScore)
  console.log(typeof score)
  console.log(typeof score.BScore)
  console.log(score.BScore)
  getNewQuestion(selected.nextQuestion)
}

function getCurrentQuestion() {
  return now.value
}

function getQuestion(){
  if(getCurrentQuestion()?.Question?.includes("$NAME")){
    console.log( getCurrentQuestion().Question.replace("$NAME", user.name))
    return getCurrentQuestion().Question.replace("$NAME", user.name);
  }else {return getCurrentQuestion().Question}
}
</script>

<template>
  <div class="m-20">
    <div class="text-3xl font-semibold m-12">PROJECT1-SEC-1-boi_046</div>
    <div class="w-full flex-col h-full justify-center">
      <img src="./assets/images/uc-icon.png" class="h-96 justify-center m-auto">
      <div class="m-auto w-96 bg-yellow-400 p-10 rounded-xl text-3xl font-extrabold text-center">Under Construction
      </div>
    </div>
<hr class="m-5">
    <button @click="getQuestion" class="btn bg-gray-700 p-2 mr-5 rounded-xl text-red-500 font-extrabold">Now
      question</button>
    <button @click="gameStart" class="btn bg-gray-700 p-2 rounded-xl text-red-500 font-extrabold">Start</button>
    <div class="font-extrabold text-5xl mt-5">{{ getQuestion() }}</div>
    <button @click="select" v-for="(choice, index) in now.Option" :key="index"
      class="btn bg-yellow-500 m-5 w-48 rounded hover:bg-orange-600" :id="choice.oid">
      [{{ choice.oid }}] {{ choice.answer }}
    </button>
    <div class="card bg-green-600 w-48 font-bold text-center rounded-md text-white p-6"
      :class="endGame == undefined ? 'hidden' : ''">{{ endGame }}
      <br><br>
      A Score : <span class="border-2 mt-5 p-1 px-2 rounded-sm border-white">{{ score.AScore }}</span>
      <br><br>
      B Score : <span class="border-2 mt-5 p-1 px-2 rounded-sm border-white">{{ score.BScore }}</span>
    </div>
  </div>

</template>

<style scoped>

</style>
