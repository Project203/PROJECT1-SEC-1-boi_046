<script setup>
import data from "./assets/data-mockup.json";
import { ref, reactive } from "vue";
console.log(data)
// function game() {
let user = reactive({ name: "Jolly Dev", ages: 23 })
let score = ref(0)
let now = ref({})
let showNextDialogBtn = ref(false)

function setPlayer(name) {
  user.name = name
}

function gameStart() {
  console.log("start...")
  setScene(1)
}

function getCurrentScene() {
  return now.value
}

function getDialog() {
  console.log("getDialog()")
  console.log(showNextDialogBtn)
  if(getCurrentScene()?.dialog?.includes("$NAME")){
    console.log( getCurrentScene().dialog.replace("$NAME", user.name))
    return getCurrentScene().dialog.replace("$NAME", user.name);
  }else {return getCurrentScene().dialog}
}


function getOption() {
  return now?.options
}

function setScene(no) {
  console.log('set scene ... ' + no)
  if (no === 0) return endScene()
  const newScene = data.find(e => e.no == no)
  console.log(newScene)
  if (newScene === undefined) {
    now.value = {}
  } else {
    console.log("successfully")
    now.value = newScene
    showNextDialogBtn = now.value.options.length===0?true:false
  }
}

function nextDialog(){
  setScene(parseInt(now.value.no)+1)
}

function selectOption(event) {
  const selected = now.value?.options.find(e => e.id == event.target.id)
  console.log(now.value.option)
  console.log(selected.next)
  score += selected?.score ?? 0
  setScene(selected.next)
}


  


//   return { setPlayer, gameStart, getCurrentScene, getDialog, getOption, setScene, selectOption }
// }

// const { setPlayer, gameStart, getCurrentScene, getDialog, getOption, setScene, selectOption } = game()

</script>

<template>

  <button @click="gameStart()" class="btn bg-gray-700 p-2 rounded-xl text-red-500 font-extrabold">Start</button>
  <div class="font-extrabold text-5xl mt-5">{{ getDialog() }}</div>
  <button @click="selectOption" v-for="(choice, index) in now.options" :key="index"
    class="btn bg-yellow-500 m-5 w-48 rounded hover:bg-orange-600" :id="choice.id">
    [{{ choice.id }}] {{ choice.message }}
  </button>

  <button @click="nextDialog()" class="btn bg-gray-700 p-2 rounded-xl text-pink-500 font-extrabold" >Next</button>
  


  <div class="card bg-green-600 w-48 font-bold text-center rounded-md text-white p-6"
    :class="endScene == undefined ? 'hidden' : ''">{{}}
    <br><br>
    A Score : <span class="border-2 mt-5 p-1 px-2 rounded-sm border-white">{{ score }}</span>
  </div>

</template>

<style scoped>

</style>
