<script setup>
import data from "./assets/data-mockup.json";
import { ref, reactive } from "vue";
console.log(data)

// user input data
let playername = ref('John Dev')

//game function
function game() {
  let user = { name: "Jolly Dev", ages: 23 }
  let score = 0
  let now = ref({})
  let nextDialogBtn = false
  let state = 1;
  
  function setPlayer(name) {
    console.log(name)
    user.name = name
  }

  function gameStart() {
    console.log("start...")
    setPlayer(playername.value)
    setScene(1)
    state = 2
  }

  function getCurrentScene() {
    return now.value
  }

  function interactiveDialogs(dialog) {
    let replaceDialog = dialog
    if(dialog==undefined) return undefined
    // console.log(replaceDialog)
    // NAME
    if (replaceDialog?.includes("$NAME")) {
      console.log(typeof replaceDialog)
      replaceDialog = replaceDialog.replace("$NAME", user.name)
     }
    return replaceDialog
  }

  function getDialog() {
    console.log("---- getDialog ----")
    console.log(nextDialogBtn)
    return interactiveDialogs(getCurrentScene()?.dialog)
  }

  function endScene() {
    state = 3
    return getEndScene()
  }

  function getEndScene(){
    return {score:score,message:"==========END==========="}
  }

  function getOption() {
    let options = []
    if (now.value?.options == undefined) return []
    console.log(now.value.options)
    now.value.options.forEach(e => {
      e.message = interactiveDialogs(e.message)
      options.push(e)
    })
    return options
  }

  function setScene(no) {
    console.log('set scene ... ' + no)
    if (no === 0)  endScene()
    const newScene = data.find(e => e.no == no)
    console.log(newScene)
    if (newScene === undefined) {
      console.log("undefscene")
      now.value = {}
    } else {
      console.log("successfully")
      now.value = newScene
      nextDialogBtn = now.value.options.length === 0 ? true : false
    }
  }

  function getCurrentState() {
    console.log(state)
    return state
  }

  function nextDialog() {
    setScene(parseInt(now.value.no) + 1)
  }

  function showNextDialogBtn() {
    console.log(nextDialogBtn)
    return nextDialogBtn
  }

  function selectOption(event) {
    const selected = now.value?.options.find(e => e.id == event.target.id)
    console.log(now.value.option)
    console.log(selected.next)
    score += selected?.score ?? 0
    setScene(selected.next)
  }






  return { setPlayer, gameStart, getCurrentScene, getDialog, getOption, setScene, selectOption, nextDialog, showNextDialogBtn, getCurrentState, getEndScene }
}

const { setPlayer, gameStart, getCurrentScene, getDialog, getOption, setScene, selectOption, nextDialog, showNextDialogBtn, getCurrentState, getEndScene } = game()

console.log(getOption())
</script>

<template>
  <div class="m-24">

    <button @click="getCurrentState()" class="btn bg-black p-2 rounded-xl text-gray-300 font-extrabold">Show State</button>
    <button @click="getCurrentState()" class="btn bg-black p-2 rounded-xl text-gray-300 font-extrabold">Show State</button>

    <div class="bg-rose-100 p-10" v-show="getCurrentState() == 1">
      <input v-model="playername" placeholder="edit me" />
      <button @click="gameStart()" class="btn bg-black text-gray-100 font-bold px-5">Start</button>
    </div>



    <div v-show="getCurrentState() == 2">
      <div class="font-extrabold text-5xl mt-5">{{ getDialog() }}</div>
      <button @click="selectOption" v-for="(choice, index) in getOption()" :key="index"
        class="btn bg-yellow-500 m-5 w-48 rounded hover:bg-orange-600" :id="choice.id">
        [{{ choice.id }}] {{ choice.message }}
      </button>

      <button @click="nextDialog()" v-show="showNextDialogBtn()"
        class="btn bg-gray-700 p-2 rounded-xl text-pink-500 font-extrabold">Next
      </button>

    </div>


    <div class="card bg-green-600 w-48 font-bold text-center rounded-md text-white p-6" v-show="getCurrentState() == 3">
      {{ getEndScene().message}}
      <br><br>
      A Score : <span class="border-2 mt-5 p-1 ml-5 px-2 rounded-sm border-white">{{ getEndScene().score }}</span>
    </div>

  </div>

</template>

<style scoped>

</style>
