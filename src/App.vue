<script setup>
import { ref, onMounted } from "vue";
import data from "./assets/data/data-mockup.json";
import summalize from "./assets/data/summalize.json";
import characterinfo from "./assets/data/characterinfo.json";
import randomname from "./assets/data/randomName.json";
// user input data
let playername = ref('ไกซ์')

// select Character
let selected = ref(null)

onMounted(() => {
  inputMusic.value.src = 'themesongs/background.mp3'
  isPlaying.value = !isPlaying.value
  playPauseSong()
})

//song
const isPlaying = ref(false)
const inputMusic = ref(null)
const musicVolume = ref(0.4)
const effect = ref(null)
const effectVolume = ref(1.0)

// btn
const setting = ref(false)
const achieve = ref(false)
const info = ref(false)
const help = ref(false)

// menu
let count = ref(0)
const displayMenu = ref(false)

const effectSound = (effectName) => {
  effect.value.src = `effects/${effectName}`
  effect.value.volume = effectVolume.value
  effect.value.play()
}
// sound control
const Effectcontrol = () => {
  effect.value.volume = effectVolume.value
}
const Musiccontrol = () => {
  inputMusic.value.volume = musicVolume.value
}
// select character
const selectCharacter = ref(false)

let currentCharacter = ref(0)


const nextCharacter = () => {
  if (currentCharacter.value === 5) {
    currentCharacter.value = 0
  } else {
    currentCharacter.value++
  }

}
const backCharacter = () => {
  if (currentCharacter.value === 0) {
    currentCharacter.value = 5
  } else {
    currentCharacter.value--
  }

}
// randomname
const randomName = () => {
  const randomed = Math.floor(Math.random() * randomname.length + 1);
  playername.value = randomname[randomed]
  effectSound('dice.mp3')
}

//game function :: closure
function game() {
  let characterName = { th: "ไข่ตุ๋น", en: "kaitoon" };
  let user = { name: "" }
  let state = ref(1);
  let score = 0
  let now = ref({ dialog: "ขออภัย คุณไม่มีสิทธิในการเข้าถึงหน้านี้ หากคิดว่าการแจ้งเตือนนี้ผิดพลาดขอให้ refresh page อีกครั้ง" })
  let characterMood = "";
  let imageBackground = "error.png";
  let endData = {};
  let history = [];

  function gameStart() {
    user.name = playername.value
    characterMood = "no.png"
    score = 6
    endData = {}
    state.value = 2
    setScene(1)
    effectSound('alarm.mp3')
  }

  function saveCharacter() {
    if (currentCharacter.value === 0) {
      characterName.value = characterinfo[0]
    }
    if (currentCharacter.value === 1) {
      characterName.value = characterinfo[1]
    }
    if (currentCharacter.value === 2) {
      characterName.value = characterinfo[2]
    }
    if (currentCharacter.value === 3) {
      characterName.value = characterinfo[3]
    }
    if (currentCharacter.value === 4) {
      characterName.value = characterinfo[4]
    }
    if (currentCharacter.value === 5) {
      characterName.value = characterinfo[5]
    }
    selectCharacter.value = false
    selected.value = 'DATE WITH ' + characterName.value.en.toUpperCase()
  }

  function setScene(no) {
    // check to endScene 
    if (no === 0) {
      if (now.value.no == 150) endScene(9)
      else endScene(0)
    }

    // set new scene
    const newScene = data.find(e => e.no == no)
    if (newScene === undefined) {
      now.value = { dialog: "Error" }
      imageBackground = "error.png"
    } else {
      now.value = newScene
      imageBackground = newScene.background
      countScene()
    }
  }

  function countScene() {
    count.value++
  }

  function interactiveDialogs(dialog) {
    let replaceDialog = dialog
    let i = 0
    while (replaceDialog?.includes("$") && i < 3) {
      i++
      if (replaceDialog?.includes("$NAME")) replaceDialog = replaceDialog.replace("$NAME", user.name)
      if (replaceDialog?.includes("$CHARNAME")) replaceDialog = replaceDialog.replace("$CHARNAME", characterName.th)
    }
    return replaceDialog
  }

  function getDialog() {
    return interactiveDialogs(now.value?.dialog)
  }

  function selectOption(id) {
    const selected = now.value?.options.find(e => e.id == id)
    score += selected?.score ?? 0
    setScene(selected.next)
    if (selected?.characterMood !== null) characterMood = selected?.characterMood
    if (selected.effect !== null) { effectSound(selected.effect) }
    else { effectSound('choice.mp3') }
    getThemesong()
    pushHistory()
    console.log(history)
  }

  function getOption() {
    let options = []
    if (now.value?.options == undefined) return []
    now.value.options.forEach(oldOption => {
      let option = { id: oldOption?.id, message: interactiveDialogs(oldOption.message) }
      options.push(option)
    })
    return options
  }

  function endScene(type) {
    state.value = 3
    let keyEnding = ""
    // end game summalize
    if (type === 0) {
      if (score > 34) {
        keyEnding = "b1"
      } else if (score > 14) {
        keyEnding = "b2"
      } else {
        keyEnding = "b3"
      }
    } else if (type === 9) {
      // endgame in no.150 summalize
      if (score > 5) {
        keyEnding = "a1"
      } else {
        keyEnding = "a2"
      }
    }
    const endObj = summalize.find(e => e.id == keyEnding)
    endData.message = interactiveDialogs(endObj.message)
    endData.score = score
    endData.image = endObj.image
    endData.endingWord = endObj.endingWord
    return getEndScene()
  }


  function getThemesong() {
    if (state.value == 3) {
      inputMusic.value.src = 'themesongs/end.mp3'
      inputMusic.value.volume = musicVolume.value
      if (isPlaying.value) inputMusic.value.play()
      else inputMusic.value.pause()
    } else if (state.value == 1) {
      inputMusic.value.src = 'themesongs/background.mp3'
      inputMusic.value.volume = musicVolume.value
      if (isPlaying.value) inputMusic.value.play()
      else inputMusic.value.pause()
    } else if (now.value.themesong === null) {
      inputMusic.value.volume = musicVolume.value
      if (isPlaying.value) inputMusic.value.play()
      else inputMusic.value.pause()
    } else {
      inputMusic.value.src = `themesongs/${now.value.themesong}`
      inputMusic.value.volume = musicVolume.value
      if (isPlaying.value) inputMusic.value.play()
      else inputMusic.value.pause()
    }
  }

  function getEndScene() {
    return endData
  }

  function getCurrentState() {
    return state.value
  }

  function showDirectorScene() {
    return now.value?.who == "director" ? true : false
  }

  function showNextDialogBtn() {
    return getOption().length == 1 ? true : false
  }

  function getName() {
    return now.value?.who == "user" ? user.name : now.value?.who == "system" ? characterName.th : now.value?.who
  }

  function getCharecterMood() {
    return `images/character/${characterName.en}/${characterMood}`
  }

  function getBackground() {
    return `images/background/${imageBackground}`
  }

  function goHomePage() {
    selected.value = null
    effectSound((state.value === 2 ? 'goHome.mp3' : 'choice.mp3'))
    state.value = 1
    isPlaying.value = !isPlaying.value
    playPauseSong()
  }

  function effectSound(effectName) {
    effect.value.src = effectName
    effect.value.volume = 0.33
    effect.value.play()
  }

  function playPauseSong() {
    isPlaying.value = !isPlaying.value
    if (isPlaying.value) getThemesong()
    else inputMusic.value.pause()
  }


  function pushHistory() {
    if (now.option.message !== ">>")
      return history.push(now.option.message)
  }

  function checkHistory() {
    return console.log(history)
  }

  return { getThemesong, playPauseSong, saveCharacter, gameStart, getDialog, getOption, selectOption, showNextDialogBtn, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene, checkHistory }
}


const { getThemesong, playPauseSong, saveCharacter, gameStart, getDialog, getOption, selectOption, showNextDialogBtn, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene, checkHistory } = game()


</script>
<template>
  <!-- Icon Web + Song ------------------------------------------------------------------------------------------------------------------------------->
  <audio ref="effect" />
  <div class="shadow-4xl bg-white pb-4 pr-2 rounded-br-[30px] absolute z-10 w-24 rounded-bl-[10px] rounded-tr-[10px]">
    <img src="./assets/images/element/Logo.png" class="scale-100" />
    <audio ref="inputMusic" id="startMusic-001" autoplay />
    <button fleid="mybtn" class="w-20 h-10 rounded-full hover:scale-[115%] duration-300 each-in-out bg-pink-500 m-1"
      @click="playPauseSong()">
      <span class="flex justify-center text-white">{{ isPlaying ? "Pause" : "Play" }}</span>
    </button>
    <!-- Setting Button -->
    <div
      class="w-20 h-10 rounded-full hover:scale-[115%] duration-300 each-in-out flex justify-center text-white bg-pink-500 m-1"
      @click="setting = !setting">
      Setting</div>
  </div>

  <!-- Setting sound v-show setting-->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center rounded-3xl z-50" v-show="setting">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Sound Settings</div>
      <div class="bg-slate-50 rounded-lg p-5 my-3">
        <div class="absolute right-[26%] top-[80px] text-5xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
          @click="setting = !setting">
          <ion-icon @click="selectCharacter = !selectCharacter" name="close-sharp"></ion-icon>
        </div>
        <div class="flex justify-between">
          <div class="text-lg font-bold">Background Music </div>
          <div class="flex justify-center"> {{ musicVolume * 100 }}% </div>
        </div>
        <input @input="Musiccontrol()" id="music" v-model="musicVolume" type="range" min="0" step="0.01" max="1"
          class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700">
      </div>
      <div class="bg-slate-50 rounded-lg p-5 my-3">
        <div class="flex justify-between">
          <div class="text-lg font-bold">Sound Effect </div>
          <div class="flex justify-center"> {{ effectVolume * 100 }}% </div>
        </div>

        <input @input="Effectcontrol()" id="effect" v-model="effectVolume" type="range" min="0" step="0.01" max="1"
          class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700">
      </div>
      <div class="bg-slate-50 rounded-lg p-5 my-3">
        <div class="flex justify-between">
          <div class="text-lg font-bold">Change Background Song </div>

        </div>
        <select id="bg-song"
          class="bg-gray-200 border mt-3 border-gray-300 text-md font-semibold rounded-lg text-slate-700  focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 ">
          <option selected>Choose a background song...</option>
          <option value="01">song 01</option>
          <option value="02">song 02</option>
          <option value="03">song 03</option>
          <option value="04">song 04</option>
        </select>

      </div>
      <div class="bg-[#f82b74] text-white rounded-full p-5 mt-24 hover:scale-105 ease-in-out duration-300"
        @click="setting = !setting">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>

  <!-- Achievement v-show setting-->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center rounded-3xl z-50" v-show="achieve">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Achievement</div>
      <div class="absolute right-[26%] top-[80px] text-5xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
        @click="achieve = !achieve">
        <ion-icon @click="selectCharacter = !selectCharacter" name="close-sharp"></ion-icon>
      </div>
      <div class="grid grid-cols-3 gap-6 mt-5">
        <div class="flex flex-col first-letter w-64 ">
          <div class="bg-white h-64 rounded-xl shadow-gray-400 shadow-xl"> </div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            ITEM#01 </div>
        </div>


      </div>
      <div class="bg-[#f82b74] text-white rounded-full p-5 mt-36 hover:scale-105 ease-in-out duration-300"
        @click="achieve = !achieve">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>

  <!-- firstpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 1">
    <img src="./assets/images/other/Enerd.png"
      class="absolute -z-20 h-[90%] left-[-10%] scale-[90%] -bottom-16 contrast-150 shadowcharecter delay-900 duration-500 ease-in-out" />
    <img src="./assets/images/other/EyenCha.png"
      class="absolute -z-40 h-[70%] left-[7%] -top-[52px] scale-[115%] contrast-150 shadowcharecter duration-500" />
    <img src="./assets/images/other/Enu.png"
      class="absolute -z-20 h-[90%] left-[15%] bottom-14 contrast-[125%] shadowcharecter" />
    <img src="./assets/images/other/Edu.png"
      class="absolute -z-10 h-[90%] -bottom-32 -left-[-17%] contrast-[125%] left shadowcharecter" />
    <img src="./assets/images/other/Ejaidee.png"
      class="absolute -z-30 h-[80%] bottom-52 -left-[12%] contrast-100 left shadowcharecter" />
    <img src="./assets/images/other/Ekaitoon.png"
      class="absolute z-0 h-[120%] -bottom-52 left-[-5%] contrast-[100%] left" />
    <img src="./assets/images/element/gameName.png" class="w-full h-full absolute -z-50" />



    <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center rounded-3xl z-50 overflow-hidden"
      v-show="selectCharacter">
      <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl">
        <div class="text-center color-black bg-white font-extrabold text-4xl rounded-lg p-5 my-3 z-50 shadow-lg bg-opacity-70">
          SELECT CHARACTER <p class="mali mt-5 px-3 text-[#f82b74] ">" N' {{ characterinfo[currentCharacter].en.toUpperCase() }} "</p>
        </div>
        <div class="absolute right-[26%] top-[80px] text-5xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
          @click="selectCharacter = !selectCharacter">
          <ion-icon name="close-sharp"></ion-icon>
        </div>

        <div class="flex ">
        <div class="flex w-20 h-10 mt-40 text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
          @click="backCharacter"> <ion-icon name="chevron-back-outline"></ion-icon></div>
        <div class="flex -mt-36 ">
          <img :src="characterinfo[currentCharacter].image" class="w-fit" />
        </div>
        <div class="flex w-20 h-10  mt-40  hover:skip text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
          @click="nextCharacter"><ion-icon name="chevron-forward-outline"></ion-icon></div>
      </div>
      <div
        class="bg-white text-5xl -mt-28 ml-[47rem] z-40 hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer flex px-5 py-3 mr-5 w-[18%] h-fit justify-center rounded-full border-[#f82b74] border-4 border-solid"
        @click="saveCharacter">
        Save
      </div>
        
        
      </div>
    </div>


    <div class="w-full h-full relative">



      <!-- Achivement -->
      
      <!-- help -->
    
      <!-- info -->

      <div class="w-1/2 h-3/5 absolute right-20 bottom-12">
        <div class="w-full h-full flex flex-col mt-6 place-items-end relative">
          <div class="h-1/6 w-1/2 rounded-full flex justify-center items-center text-3xl mali text-[#9B4F5E] font-bold">
            <div>
              <input placeholder="Enter Your Name" v-model="playername" maxlength="18" required
                class="text-center boi-input focus:border-[#9B4F5E] rounded-tl-3xl  rounded-br-3xl h-16 mr-5">
            </div>
            <div @click="randomName"
              class="transition delay-100 hover:scale-[115%] duration-200 each-in-out cursor-pointer flex justify-center place-items-center text-5xl rounded-full p-3 border-[#f82b74] border-4 border-solid bg-white absolute right-0">
              <ion-icon name="dice-outline"></ion-icon>
            </div>
          </div>

          <div @click="selectCharacter = !selectCharacter"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-5 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            {{ selected === null ? 'SELECT CHARACTER' : selected }}
            <div class="pl-4 flex items-center">
              <ion-icon name="heart-half-sharp"></ion-icon>
            </div>
          </div>
          <div @click="gameStart"
            :style="selected === null ? 'pointer-events:none; color:#D3D3D3; border-color:#D3D3D3; background-color:grey' : ''"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-8 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            {{ selected === null ? 'SELECT CHARACTER FIRST!!' : 'START GAME' }}
            <div class="pl-4 flex items-center">
              <ion-icon name="arrow-forward-sharp"></ion-icon>
            </div>
          </div>
          <div @click="setting = !setting"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-8 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            SETTING
            <div class="pl-4 flex items-center">
              <ion-icon name="settings-sharp"></ion-icon>
            </div>
          </div>
          <div @click="achieve = !achieve"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-8 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            ACHIEVEMENT
            <div class="pl-4 flex items-center">
              <ion-icon name="ribbon-sharp"></ion-icon>
            </div>
          </div>
          <div class="mt-5 h-1/6 w-96 ml-64 grid grid-cols-3 gap-20 p-4">
            <div @click="help = !help"
              class="text-4xl bg-white rounded-full border-[#f82b74] border-4 border-solid flex justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="help-sharp"></ion-icon>
            </div>
            <div @click="info = !info"
              class="text-4xl bg-white rounded-full flex border-[#f82b74] border-4 border-solid justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="information-sharp"></ion-icon>
            </div>
            <div @click=""
              class="text-4xl bg-white rounded-full border-[#f82b74] border-4 border-solid flex justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="share-social-sharp"></ion-icon>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- subtitles------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 2">
    <div class="w-full h-24 flex absolute">
      <div class="w-30 w-full flex justify-end">
        <!-- Back Btn -->
        <div
          class="z-50 mr-auto ml-36 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          Scene: {{ count }}
        </div>
        <div @click="displayMenu = !displayMenu"
          class="focus:bg-rose-600 z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          MENU
        </div>
        <div @click="checkHistory"
          class="z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          HISTORY
        </div>
        <div @click="goHomePage"
          class="z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          RESTART
        </div>
      </div>
    </div>

    <!-- menu-->
    <div v-show="displayMenu" class="w-[90%] h-[90%]  -z-10 bg">
      <div>

      </div>

    </div>

    <!-- cutScene -->
    <div v-show="showDirectorScene()" class="w-full h-full page-change z-10">
      <img :src="getBackground()" class=" absolute w-full h-full -z-50 " />
      <div class="w-full h-full flex justify-center text-xl font-semibold mali items-center"
        @click="selectOption(getOption()[0].id)">
        <div
          class="relative w-[50%] h-[75%] border-[#f82b74] border-4 flex mr-auto ml-auto indent-10 p-20 items-center bg-white bg-opacity-70 rounded-bl-[100px] rounded-tr-[100px] show-dialog">
          <p class="pb-32 leading-[2em] text-[#f82b74] typing break-words text-2xl indent-16">{{ getDialog() }}</p>
          <div v-show="true" class="z-50 bounce absolute cursor-pointer text-2xl w-20 h-20 bottom-4 right-8">
            <img src="./assets/images/element/skipwhite.png">
          </div>
        </div>
      </div>
    </div>

    <!-- gameplaypage--------------->
    <div class="w-screen h-screen page-change" v-show="!showDirectorScene()">
      <!-- background image -->
      <img :src="getBackground()" class="absolute -z-50 w-full h-full justify-center" />
      <!-- header bar -->
      <div class="w-full h-full translation-fade-1">
        <!-- game content -->
        <div class="w-full h-[75%] flex">
          <!-- left + charecter-->
          <div class=" w-full flex justify-center relative -z-50">
            <!-- image -->
            <div class="w-full -top-20 flex justify-center absolute ">
            </div>
            <img :src="getCharecterMood()" class="scale-[175%] -z-50 pr-28" />
          </div>
          <!-- right + option -->
          <div class="w-8/12 grid text-[#f82b74] font-semibold text-3xl mali pt-20 pb-20 show-option"
            v-show="!showNextDialogBtn()">
            <!-- choice -->
            <div v-for="(option, index) in getOption()" :key="index" @click="selectOption(option.id)"
              v-show="option.id !== null"
              class="opacity-80 hover:opacity-100 mr-12 break-all w-fit hover:bg-[#f82b74] hover:border-white hover:scale-[105%] duration-300 each-in-out hover:text-white cursor-pointer border-[#f82b74] border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-12 pr-12 bg-fuchsia-50">
              {{ option.message }}
            </div>
          </div>
        </div>
        <!-- footer -->
        <div
          class="w-full h-64 flex justify-center text-white mali text-2xl font-semibold bg-rose-400 bg-opacity-80 border-t-4 border-white rounded-tl-[20px] show-dialog">
          <div class="w-10/12 relative ">
            <!-- dialog -->
            <p class="ml-20 mt-12 mr-28 pt-5"> {{ getDialog() }}
            </p>
            <!-- name -->
            <div
              class=" border-white border-4 -top-9 h-16 absolute ml-28 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-500 rounded-3xl drop-shadow-3xl">
              <p class="flex text-center"> {{ getName() }} </p>
            </div>
            <!-- next dialog btn -->
            <div v-show="showNextDialogBtn()" @click="selectOption(getOption()[0].id)"
              class="bounce cursor-pointer text-2xl  w-20 h-20 m-1 absolute bottom-5 right-16 mali flex place-items-center justify-center skip">
              <img src="./assets/images/element/skipwhite.png">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- endingpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 3">
    <img src="./assets/images/backEnd.png" class="absolute -z-50 w-full h-full justify-center" />
    <div class="w-full h-full p-1 flex">
      <!-- left -->
      <div class="w-1/2 h-[90%] mt-20">
        <div class="w-5/6 h-[70%] mt-8 ml-auto mr-auto m-1">
          <!-- Ending Image Character -->
          <img class=" -z-50 w-full h-full border-red-300 border-2 rounded-3xl bg-red-200 bg-opacity-70"
            :src="getEndScene().image">
        </div>
        <div class="w-5/6 h-1/3 mt-4 ml-auto mr-auto m-1 justify-center flex items-center">
          <!-- Ending -->
          <div class="mali flex text-7xl font-bold border-red-300 border-2 rounded-3xl bg-red-200 px-14 py-5 -mt-[260px]">
            {{ getEndScene().endingWord }}</div>

        </div>
      </div>
      <!-- right -->
      <div class="w-1/2 ml-1 h-[90%] ">
        <!-- score -->
        <div
          class="mali w-3/4 h-[30%] m-1 mt-24 mr-auto ml-auto text-7xl flex justify-center	relative border-2 bg-rose-50 rounded-3xl drop-shadow-3xl bg-opacity-80">
          <div
            class="flex  justify-center -top-8 h-16 -left-10 absolute mali border-red-300 border-2 border-solid font-bold place-items-center px-8 text-3xl bg-[#f82b74] rounded-3xl drop-shadow-3xl ">
            <p class="text-white font-bold "> Total Score </p>
          </div>
          <p class="flex justify-center items-center mb-5 ">{{ getEndScene().score }} point(s)</p>
        </div>

        <!-- ending text -->
        <div
          class=" w-3/4 h-[45%] mt-16 mr-auto ml-auto border-red-300 border-2 relative flex justify-center bg-rose-50 rounded-3xl bg-opacity-80">
          <div
            class="text-white mali border-red-300 border-2 border-solid font-bold -top-8 h-16 absolute -left-10 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-[#f82b74] rounded-3xl drop-shadow-3xl">
            <p class="flex text-center text-semibold"> บทสรุปของ "{{ playername }}" </p>
          </div>
          <p class="mr-10 ml-10 mt-14 text-2xl mali indent-10 font-semibold break-words leading-10">
            {{ getEndScene().message }}
          </p>
        </div>
      </div>
      <div @click="goHomePage"
        class="absolute bounce z-30 bottom-4 right-4 text-3xl mali font-semibold text-[#f82b74] cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full p-1 bg-fuchsia-50 retry-btn">
        <div class="bg-[#ffffff] rounded-full py-3 pl-8 pr-8 ">Retry</div>
      </div>
    </div>
  </div>
</template>
<style scoped>
body {
  background-color: #000000 !important;
}

.mali {
  font-family: "Mali", cursive;
}

.boi-input {
  padding: 12px 20px;
  box-sizing: border-box;
  border: 4px solid #f82b74;
  -webkit-transition: 0.5s;
  transition: 0.5s;
  outline: none;
}

.left {
  transform: scaleX(-1);
}

.bounce {
  animation: bounce 1s infinite;
}

@keyframes bounce {

  0%,
  100% {
    transform: translateX(-20%);
    animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
  }

  50% {
    transform: translateX(0);
    animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
  }
}

.shadowcharecter {
  animation: shadowchar 1s infinite;
}

@keyframes shadowchar {

  0%,
  100% {
    transform: translateY(1%);
    animation-timing-function: cubic-bezier(-0.1, 0, 1, 0.8);
  }

  50% {
    transform: translateY(0.1%);
    animation-timing-function: cubic-bezier(0.2, 0.1, 0.8, 1);
  }
}

.page-change {
  animation: pageTransitionFade 1s ease-in-out;
}

@keyframes pageTransitionFade {

  50% {

    background-color: #b10239;
    opacity: 0.8;
  }

  0%,
  100% {
    transform: scale(100%);
    opacity: 1;
  }
}

.skip {
  animation: skip-annimation 1.2s infinite ease-in;
}

@keyframes skip-annimation {
  50% {
    transform: translateX(10%)
  }
}

.show-option {
  animation: option-annimation 1s ease-in-out;
}

@keyframes option-annimation {
  from {
    transform: translateX(10%);
    opacity: 0.1;
  }

  to {
    transform: translateX(0)
  }
}

.show-dialog {
  animation: dialog-bar-annimation 1s ease-in-out;
}

@keyframes dialog-bar-annimation {
  from {
    transform: translateY(5%);
    opacity: 10%;
  }

  to {
    transform: translateX(0)
  }
}

.retry-btn {
  background: linear-gradient(60deg, #ef4e7b, #a166ab, #5073b8, #1098ad, #088877);
  animation: animatedbordergradient 3s ease-in-out alternate infinite;
  background-size: 300% 300%;
}

@keyframes animatedbordergradient {
  0% {
    background-position: 0% 50%;
    bottom: 20px;
  }

  50% {
    background-position: 100% 50%;
    bottom: 15px;
  }

  100% {
    background-position: 0% 50%;
    bottom: 20px;
  }
}

.typing {
  overflow: hidden;
  /* Ensures the content is not revealed until the animation */
  white-space: break-spaces;
  /* Keeps the content on a single line / / Gives that scrolling effect as the typing happens */
  letter-spacing: .05em;
  /* Adjust as needed */
  animation:
    typing 1s steps(1500, end),
    blink-caret .75s step-end infinite;
}


/* The typing effect */
@keyframes typing {
  from {
    width: 0
  }

  to {
    width: 100%
  }

  /* The typewriter cursor effect */
}

@keyframes blink-caret {

  from,
  to {
    border-color: transparent
  }

  50% {
    border-color: rgb(255, 255, 255);
  }
}
</style>


