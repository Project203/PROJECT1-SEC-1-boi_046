<script setup>
import { ref } from "vue";
import data from "./assets/data/data-mockup.json";
import summalize from "./assets/data/summalize.json";
// user input data
console.log(summalize)
let playername = ref('บะแบงค์คึ')
//game function
function game() {
  let user = { name: "", ages: 20 }
  let score = 0
  let now = ref({ dialog: "ขออภัย คุณไม่มีสิทธิในการเข้าถึงหน้านี้ หากคิดว่าการแจ้งเตือนนี้ผิดพลาดขอให้ refresh page อีกครั้ง" })
  let nextDialogBtn = false
  let state = ref(1);
  let characterName = { th: "ขะไข่ตุ๋นนึ", en: "kaitoon" }
  let characterMood = ""
  let imageBackground = "error.png"
  let endData = {}
  let directorScene = false

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
    if (replaceDialog?.includes("$CHARNAME")) {
      replaceDialog = replaceDialog.replace("$CHARNAME", characterName.th)
      console.log("charector")
    }
    console.log(replaceDialog)
    return replaceDialog
  }
  function getDialog() {
    return interactiveDialogs(now.value?.dialog)
  }
  function endScene(type) {
    state.value = 3
    let keyEnding = ""
    if (type === 0) {
      if (score > 34) {
        keyEnding = "b1"
      } else if (score > 14) {
        keyEnding = "b2"
      } else {
        keyEnding = "b3"
      }
    } else if (type === 9) {
      if (score > 5) {
        keyEnding = "a1"
      } else {
        keyEnding = "a2"
      }
    }
    endData = summalize.find(e => e.id = "a1")
    endData.message = interactiveDialogs(endData.message)
    endData.score = score
    return getEndScene()
  }
  function getEndScene() {
    return endData
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
    if (no === 0) endScene(0)
    // check score halfway to endScene or continous
    if (now.value.no == 9 && score > 100) endScene(9)
    const newScene = data.find(e => e.no == no)
    if (newScene === undefined) {
      now.value = { dialog: "Error" }
      imageBackground = "error.png"
    } else {
      now.value = newScene
      imageBackground = newScene.background
      directorScene = newScene.who == "director" ? true : false
      console.log(directorScene)
    }
  }

  function showDirectorScene() {
    return directorScene
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
    if (selected?.characterMood !== null) characterMood = selected?.characterMood
  }

  function getName() {
    return now.value?.who == "user" ? user.name : characterName.th
  }

  function getCharecterMood() {
    return `images/character/${characterName.en}/${characterMood}`
  }

  function getBackground() {
    return `images/background/${imageBackground}`
  }

  function goHomePage() {
    state.value = 1
  }

  return { gameStart, getDialog, getOption, selectOption, showNextDialog, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene }
}
const { gameStart, getDialog, getOption, selectOption, showNextDialog, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene } = game()
</script>

<template>
  <div class="shadow-4xl bg-white pb-4 pr-2 rounded-br-[30px] absolute z-10 w-24 rounded-bl-[10px] rounded-tr-[10px]">
    <img src="./assets/images/element/Logo.png" class="scale-100" />
  </div>

  <!-- firstpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen " v-if="getCurrentState() == 1">
    <img src="./assets/images/element/backGroudGame.jpg" class="absolute z-0 w-full h-full justify-center flex" />
    <div class="w-full h-full relative">
      <!-- content right -->
      <div class="w-30  w-1/12 ">

      </div>
      <!-- <img src="./assets/images/element/jingjung.png"
        class="scale-150 -top-8 absolute right-48 cursor-pointer" /> -->
      <div class="w-1/2 h-2/3 absolute right-0 bottom-12">
        <div class="w-full h-full flex flex-col">
          <div
            class="h-1/6 w-1/2 ml-52 rounded-full flex justify-center items-center text-4xl mali mt-52 text-[#9B4F5E] font-bold">
            <input placeholder="Enter Your Name" v-model="playername" maxlength="18"
              class="text-center boi-input focus:border-[#9B4F5E] rounded-tl-3xl rounded-br-3xl h-24" />
          </div>
          <div @click="gameStart()"
            class="mali hover:scale-[115%] duration-300 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer m-16 border-[#f82b74] border-8 border-solid h-1/5 w-96 ml-64 rounded-full flex justify-center items-center text-5xl bg-white">
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

  <!-- subtitles------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-show="getCurrentState() == 2">
    <div v-show="showDirectorScene()" class="w-full h-full">
      <!-- cutScene -->
      <img :src="getBackground()" class=" absolute w-full h-full -z-50 " />
      <div class="w-full h-full flex justify-center text-xl font-semibold mali">
        <div
          class="relative w-[50%] h-full border-[#f82b74] border-2 flex mr-auto ml-auto indent-10 p-20 items-center bg-white bg-opacity-75 rounded-bl-[100px] rounded-tr-[100px]">
          <p class="pb-32 leading-10 text-[#f82b74]">{{   getDialog() }}</p>
          <div v-show="true" @click="selectOption(getOption()[0].id)"
            class="z-50 absolute duration-300 each-in-out hover:scale-125 cursor-pointer text-2xl w-20 h-20 bottom-4 right-8">
            <img src="./assets/images/element/skipwhite.png">
          </div>
        </div>
      </div>
    </div>
    <!-- gameplaypage------------------------------------------------------------------------------------------------------------------------------->
    <div class="w-screen h-screen " v-show="!showDirectorScene()">
      <!-- background image -->
      <img :src="getBackground()"
        class="absolute -z-50 w-full h-full justify-center" />
      <!-- header bar -->
      <div class="w-full h-full">
        <div class="w-full h-24 flex flex-row">
          <div class="w-30 w-full flex justify-end">
            <!-- Back Btn -->
            <div @click="goHomePage()"
              class="z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
              ไออ่อนย้อนเวลา
            </div>

          </div>
        </div>
        <!-- game content -->
        <div class="w-full h-2/3 flex">
          <!-- left -->
          <div class=" w-full flex justify-center relative left -z-50">
            <!-- image -->
            <div class="w-full -top-20 flex justify-center absolute ">
            </div>
            <img :src="getCharecterMood()" class="scale-[175%] -z-50 pr-28"/>
          </div>
          <!-- right -->
          <div
            class="w-8/12 grid text-[#f82b74] font-semibold text-3xl mali pt-20 pb-20 "
            v-if="!showNextDialog()">
            <!-- choice -->
            <div v-for="(choice, index) in getOption()" :key="index" @click="selectOption(choice.id)"
              class="mr-12 break-all w-fit hover:bg-[#f82b74] hover:border-white hover:scale-[115%] duration-300 each-in-out hover:text-white cursor-pointer border-[#f82b74] border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-12 pr-12 bg-fuchsia-50">
              {{ choice.message }}
            </div>
          </div>
        </div>
        <!-- footer -->
        <div
          class="border-red-300 border-2 border-solid w-full h-56 flex justify-center text-white mali text-2xl font-semibold bg-rose-400 bg-opacity-50">
          <div class="border-red-300 border-2 border-solid w-10/12 m-1 relative ">
            <!-- dialog -->
            <p class="ml-20 mt-12 mr-28"> {{ getDialog() }}
            </p>
            <!-- name -->
            <div
              class="cursor-pointer border-red-300 border-2 -top-11 h-16 absolute ml-28 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-300 rounded-3xl drop-shadow-3xl">
              <p class="flex text-center"> {{ getName() }} </p>
            </div>
            <!-- next dialog btn -->
            <div v-show="showNextDialog()" @click="selectOption(getOption()[0].id)"
              class="duration-300 each-in-out hover:scale-125 cursor-pointer text-2xl border-red-300 border-2 border-solid w-20 h-20 m-1 absolute bottom-0 right-0 mali flex place-items-center justify-center text-white">
              <img src="./assets/images/element/skipwhite.png">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- endingpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 3">
    <div class="w-full border-red-300 border-2 h-full p-1 flex">
      <!-- left -->
      <div class="w-1/2 border-red-300 border-2 h-[90%] ">
        <div class="w-5/6 h-1/2 border-red-300 border-2 mt-24 ml-auto mr-auto m-1">
          <!-- Ending Image Character -->
          <img src="">
        </div>
        <div class="w-5/6 h-1/3 border-red-300 border-2 mt-4 ml-auto mr-auto m-1">
          <!-- Ending -->
          <img src="">
        </div>
      </div>
      <!-- right -->
      <div class="w-1/2 ml-1 h-[90%]  border-red-300 border-2 ">
        <!-- score -->
        <div class="w-3/4 h-[30%] m-1 mt-24 mr-auto ml-auto border-red-300 border-2">
          {{ getEndScene().score }}
        </div>
        <!-- ending text -->
        <div class="w-3/4 h-[45%] mt-16 mr-auto ml-auto border-red-300 border-2 relative flex justify-center">
          <div
            class="mali border-red-300 border-2 border-solid -top-8 h-16 absolute -left-10 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-300 rounded-3xl drop-shadow-3xl">
            <p class="flex text-center"> {{ getName() }} </p>
          </div>
          <p class="mr-10 ml-10 mt-12 text-lg mali indent-10">
            {{ getEndScene().message }}
          </p>
        </div>
      </div>
      <div @click="goHomePage()"
        class="absolute bottom-4 right-4 text-3xl mali font-semibold text-[#f82b74] hover:bg-rose-600 hover:text-white cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-8 pr-8 bg-fuchsia-50">
        Retry
      </div>
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
.left{
  transform: scaleX(-1);
}
</style>