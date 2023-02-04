<script setup>
import { ref } from "vue";
import data from "./assets/data/data-mockup.json";
// user input data
let playername = ref('Geist คุงงงงงง')
//game function
function game() {
  let user = { name: "", ages: 20 }
  let score = 0
  let now = ref({ dialog: "ขออภัย คุณไม่มีสิทธิในการเข้าถึงหน้านี้ หากคิดว่าการแจ้งเตือนนี้ผิดพลาดขอให้ refresh page อีกครั้ง" })
  let nextDialogBtn = false
  let state = ref(1);
  let characterName = { th: "ไข่ตุ๋น", en: "kaitoon" }
  let characterMood = ""
  let imageBackground = "error.png"

  function setPlayer(name) {
    user.name = name
  }
  function gameStart() {
    setPlayer(playername.value)
    characterMood = "base.png"
    imageBackground = "pavilion.png"
    setScene(1)
    state.value = 2
    score = 0
  }
  function interactiveDialogs(dialog) {
    let replaceDialog = dialog
    if (dialog == undefined) return undefined

    if (replaceDialog?.includes("$NAME")) {
      replaceDialog = replaceDialog.replace("$NAME", user.name)
    }
    return replaceDialog
  }
  function getDialog() {
    return interactiveDialogs(now.value?.dialog)
  }
  function endScene() {
    state.value = 3
    return getEndScene()
  }
  function getEndScene() {
    return { score: score, message: "==========END===========" }
  }
  function getOption() {
    let options = []
    if (now.value?.options == undefined) return []
    now.value.options.forEach(e => {
      e.message = interactiveDialogs(e.message)
      options.push(e)
    })
    return options
  }

  function setScene(no) {
    if (no === 0) endScene()
    // check score halfway to endScene or continous
    if(now.value.no == 9 && score > 100) endScene()
    const newScene = data.find(e => e.no == no)
    if (newScene === undefined) {
      now.value = { dialog: "Error" }
      imageBackground = "error.png"

    } else {
      now.value = newScene
      imageBackground = newScene.background
    }
  }

  function getCurrentState() {
    return state.value
  }

  function showNextDialog() {
    nextDialogBtn = getOption().length == 1 ? true : false
    return nextDialogBtn
  }

  function selectOption(id) {
    const selected = now.value?.options.find(e => e.id == id)
    console.log(selected.message)
    score += selected?.score ?? 0
    setScene(selected.next)
    if(selected?.characterMood!==null) characterMood = selected?.characterMood
  }

  function getName() {
    return now.value?.who == "user" ? user.name : characterName.th
  }

  function getCharecterMood() {
    return new URL(`/src/assets/images/character/${characterName.en}/${characterMood}`, import.meta.url)
  }

  function getBackground() {
    return new URL(`/src/assets/images/background/${imageBackground}`, import.meta.url)
  }

  function goHomePage() {
    state.value = 1
  }

  return { gameStart, getDialog, getOption, selectOption, showNextDialog, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground }
}
const { gameStart, getDialog, getOption, selectOption, showNextDialog, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground } = game()
</script>

<template>
  <div class="shadow-4xl bg-white pb-4 pr-2 rounded-br-[30px] absolute z-10 w-24 rounded-bl-[10px] rounded-tr-[10px]">
    <img src="./assets/images/element/Logo.png" class="scale-100" />
  </div>

  <!-- firstpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen " v-if="getCurrentState() == 1">
    <img src="./assets/images/background/backGroudGame.jpg" class="absolute z-0 w-full h-full justify-center flex" />
    <div class="w-full h-full relative">
      <!-- content right -->
      <div class="w-30  w-1/12 ">

      </div>
      <!-- <img src="./assets/images/element/jingjung.png"
        class="scale-150 -top-8 absolute right-48 cursor-pointer" /> -->
      <div class="w-1/2 h-2/3 absolute right-0 bottom-12">
        <div class="w-full h-full flex flex-col">
          <div
            class="h-1/6 w-1/2 ml-52 rounded-full flex justify-center items-center text-4xl mali mt-36 text-[#9B4F5E] font-bold">
            <input placeholder="Enter Your Name" v-model="playername" maxlength="18"
              class="text-center boi-input focus:border-[#9B4F5E] rounded-tl-3xl rounded-br-3xl h-24" />
          </div>
          <div @click="gameStart()"
            class="mali boi-c font-bold hoverbgc hover:text-white transition delay-100 hover:border-white cursor-pointer m-16 boi-border border-8 border-solid h-1/5 w-96 ml-64 rounded-full flex justify-center items-center text-5xl bg-white">
            PLAY NOW!!!
          </div>
        </div>
      </div>
      <div class="w-full h-8 pr-4 bottom-0 absolute flex place-items-center justify-end">
        <p class="copyright">
          © Created By KMUTT Student For Subject INT203 Client-Side
          Programming II
        </p>
      </div>
    </div>
  </div>

  <!-- gameplaypage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 2">
    <!-- background image -->
    <img :src="getBackground()" class="absolute -z-50 w-full h-full justify-center flex" />
    <!-- header bar -->
    <div class="w-full h-full border-red-300 border-2 border-solid">
      <div class="w-full border-red-300 border-2 border-solid h-24 mb-1 flex flex-row">
        <div class="w-30 border-red-300 border-2 border-solid w-full m-1 flex justify-end">
          <!-- Back Btn -->
          <div @click="goHomePage()"
            class="z-50 text-2xl mali font-semibold boi-c mt-4 mr-12 break-all w-fit hover:bg-rose-600 hover:text-white cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-8 pr-8 bg-fuchsia-50">
            ไออ่อนย้อนเวลา
          </div>

        </div>
      </div>
      <!-- game content -->
      <div class="w-full h-2/3 border-red-300 border-2 border-solid flex mb-1">
        <!-- left -->
        <div class="border-red-300 border-2 border-solid w-full m-1 flex justify-center relative">
          <!-- image -->
          <div class="border-red-300 border-2 border-solid w-full h-screen -top-20 flex justify-center absolute"></div>
          <img :src="getCharecterMood()" class="scale-150 -z-50" />
        </div>
        <!-- right -->
        <div
          class="border-red-300 border-2 border-solid w-8/12 m-1 grid text-rose-500 font-semibold text-2xl mali pt-16 pb-16"
          v-if="!showNextDialog()">
          <!-- choice -->
          <div v-for="(choice, index) in getOption()" :key="index" @click="selectOption(choice.id)"
            class="mr-12 break-all w-fit hover:bg-rose-600 hover:text-white cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-12 pr-12 bg-fuchsia-50">
            {{ choice.message }}
          </div>
        </div>
      </div>
      <!-- footer -->
      <div
        class="border-red-300 border-2 border-solid w-full h-56 flex justify-center text-white mali text-2xl font-semibold bg-white-blur bg-rose-400">
        <div class="border-red-300 border-2 border-solid w-10/12 m-1 relative">
          <!-- dialog -->
          <p class="ml-20 mt-12 mr-28"> {{ getDialog() }}
          </p>
          <!-- name -->
          <div
            class="cursor-pointer border-red-300 border-2 border-solid -top-11 h-16 absolute ml-28 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-300 rounded-3xl drop-shadow-3xl">
            <p class="flex text-center"> {{ getName() }} </p>
          </div>
          <!-- next dialog btn -->
          <div v-show="showNextDialog()" @click="selectOption(getOption()[0].id)"
            class="cursor-pointer text-2xl border-red-300 border-2 border-solid w-24 h-24 m-1 absolute bottom-0 right-0 mali flex place-items-center justify-center text-white">
            <img src="./assets/images/element/skipwhite.png">
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- endingpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 3">
    <div v-on:click="goHomePage()"
      class="cursor-pointer border-red-300 border-2 border-solid top-11 h-16 absolute ml-28 flex place-items-center p-24 text-3xl bg-rose-300 rounded-3xl drop-shadow-3xl">
      {{ getEndScene().message }}
      <span class="m-5 p-3 rounded bg-red-500 text-white"> SCORE :{{ getEndScene().score }}</span>
    </div>
  </div>
</template>

<style scoped>
.mali {
  font-family: "Mali", cursive;
}

.boi-input {
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  border: 5px solid #f82b74;
  -webkit-transition: 0.5s;
  transition: 0.5s;
  outline: none;
}

.boi-focus {
  border: 3px solid rgb(172, 5, 5);
}

.boi-bgc {
  background-color: #f82b74;
}

.hoverbgc:hover {
  background-color: #f82b74;
}

.boi-c {
  color: #f82b74;
}

.boi-border {
  border-color: #f82b74;
}
</style>