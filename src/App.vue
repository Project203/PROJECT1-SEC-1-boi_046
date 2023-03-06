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
  inputMusic.value.loop = true
  inputMusic.value.volume = musicVolume.value
  isPlaying.value = !isPlaying.value
  playPauseSong()
})

//song
const isPlaying = ref(false)
const inputMusic = ref(null)
const musicVolume = ref(0.1)
const effect = ref(null)
const effectVolume = ref(0.5)
const selectsong = ref(0)



const setsong = () => {
  inputMusic.value.volume = musicVolume.value
  if (selectsong.value == 1) {
    inputMusic.value.src = 'themesongs/background.mp3'
  }
  else if (selectsong.value == 2) {
    inputMusic.value.src = 'themesongs/marryme.mp3'
  }
  else if (selectsong.value == 3) {
    inputMusic.value.src = 'themesongs/ghostrifter.mp3'
  }
  else if (selectsong.value == 4) {
    inputMusic.value.src = 'themesongs/spring.mp3'
  }
  else if (selectsong.value == 5) {
    inputMusic.value.src = 'themesongs/matsuri.mp3'
  }
  else if (selectsong.value == 6) {
    inputMusic.value.src = 'themesongs/aot1.mp3'
  }
  else if (selectsong.value == 7) {
    inputMusic.value.src = 'themesongs/aot2.mp3'
  }
  else if (selectsong.value == 8) {
    inputMusic.value.src = 'themesongs/kickback.mp3'
  } 
  else {
    inputMusic.value.src = 'themesongs/background.mp3'
  }
}

const saveBackgroundsong = () => {
  getThemesong()
  effectSound()
}


// btn
const setting = ref(false)
const achieve = ref(false)
const info = ref(false)
const help = ref(false)
const history = ref(false)
const menu = ref(false)
// menu
let count = ref(0)
const displayMenu = ref(false)

const historyMenu = () => {
  history.value = !history.value
  menu.value = !menu.value
  effectSound()
}

const settingMenu = () => {
  setting.value = !setting.value
  menu.value = !menu.value
  effectSound()
}

const effectSound = (effectName) => {
  effect.value.src = `effects/${effectName}`
  if(effectName === null || effectName === undefined) effect.value.src = `effects/drop.mp3`
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

const currentCharacter = ref(0)

const nextCharacter = () => {
  if (currentCharacter.value === 5) {
    currentCharacter.value = 0
  } else {
    currentCharacter.value++
  }
  effectSound()
}
const backCharacter = () => {
  if (currentCharacter.value === 0) {
    currentCharacter.value = 5
  } else {
    currentCharacter.value--
  }
  effectSound()
}

// How to play
const currentHowto = ref(0)
const Howtoimage = [
{image:"images/help/simple.jpg"}, 
{image: "images/help/speak.jpg"},
{image: "images/help/shy.jpg"},
{image: "images/help/confi.jpg"},
{image: "images/help/boring.jpg"}
]
const showHelp = () =>{
  help.value = !help.value
  currentHowto.value = 0
  effectSound()
}
const nextHowto = () => {
    if (currentHowto.value === 4) {
      currentHowto.value = 0
    } else {
      currentHowto.value++
    }
    effectSound()
  }
const backHowto = () => {
    if (currentHowto.value === 0) {
      currentHowto.value = 4
    } else {
      currentHowto.value--
    }
    effectSound()
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
  let historyArr = [];
  let endAchieve = ['0', '1', '2', '3', '4']
  let keyEnding = ""

  function gameStart() {
    user.name = playername.value
    characterMood = "no.png"
    score = 6
    endData = {}
    state.value = 2
    setScene(1)
    effectSound('alarm.mp3')
    count.value = 1
    historyArr= []
  }

  function achieveEnding(keyEnding) {
    if (keyEnding == 'a1') {
      endAchieve[0] = endData.endingWord
    }
    if (keyEnding == 'a2') {
      endAchieve[1] = endData.endingWord
    }
    if (keyEnding == 'b1') {
      endAchieve[2] = endData.endingWord
    }
    if (keyEnding == 'b2') {
      endAchieve[3] = endData.endingWord
    }
    if (keyEnding == 'b3') {
      endAchieve[4] = endData.endingWord
    }
    return endAchieve
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
    selectCharacter.value = !selectCharacter.value
    selected.value = 'DATE WITH ' + characterName.value.en.toUpperCase()
    effectSound()
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
      if (replaceDialog?.includes("$CHARNAME")) replaceDialog = replaceDialog.replace("$CHARNAME", characterName.value.th)
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
    if (selected.effect !== null) {effectSound(selected.effect)}  
    else {effectSound('drop.mp3')}
    checkThemesong()
    pushHistory(selected.message)
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
    // let keyEnding = ""
    // end game summalize
    if (type === 0) {
      if (score > 40) {
        keyEnding = "b1"
      } else if (score > 24) {
        keyEnding = "b2"
      } else {
        keyEnding = "b3"
      }
    } else if (type === 9) {
      // endgame in no.150 summalize
      if (score > 12) {
        keyEnding = "a1"
      } else {
        keyEnding = "a2"
      }
    }
    const endObj = summalize.find(e => e.id == keyEnding)
    endData.message = interactiveDialogs(endObj.message)
    endData.score = score
    // endData.image = endObj.image
    if(keyEnding == 'a1' || keyEnding == 'a2') endData.image = `images/character/${characterName.value.en}/base.png`
    if(keyEnding == 'b1' || keyEnding == 'b2') endData.image = `images/character/${characterName.value.en}/smile.png`
    if (keyEnding == 'b3') endData.image = `images/character/${characterName.value.en}/shy.png`
    endData.endingWord = endObj.endingWord
    achieveEnding(keyEnding)
    return getEndScene()
  }

  function checkThemesong() {
    now.value.themesong === null ? now.value.themesong = null : getThemesong()
  }

  function getThemesong() {
    setsong()
    inputMusic.value.volume = musicVolume.value
    if (state.value === 1) {
    }
    else if (now.value.themesong === null) {
    } 
    else if (state.value === 3 && (keyEnding == 'a1' || keyEnding == 'a2')) {
      inputMusic.value.src = 'themesongs/end.mp3'
    } 
    else if (state.value === 3 && (keyEnding !== 'a1' || keyEnding !== 'a2')) {
      inputMusic.value.src = 'themesongs/kawai.mp3'
    } 
    else {
      inputMusic.value.src = `themesongs/${now.value.themesong}`
    }
    if (isPlaying.value) inputMusic.value.play()
    else inputMusic.value.pause()

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
    return now.value?.who == "user" ? user.name : now.value?.who == "system" ? characterName.value.th : now.value?.who
  }

  function getCharecterMood() {
    return `images/character/${characterName.value.en}/${characterMood}`
  }

  function getBackground() {
    return `images/background/${imageBackground}`
  }

  function goHomePage() {
    // selected.value = null
    effectSound((state.value === 2 ? 'nakomrestart.mp3' : 'drop.mp3'))
    state.value = 1
    isPlaying.value = !isPlaying.value
    setsong()
    playPauseSong()
    menu.value = false
  }

  function playPauseSong() {
    isPlaying.value = !isPlaying.value
    if (isPlaying.value) inputMusic.value.play()
    else inputMusic.value.pause()
  }


  function pushHistory(message) {
    if (message !== ">>")
      return historyArr.push(message)
  }

  function getHistory() {
    return historyArr
  }

  return { achieveEnding, getThemesong, playPauseSong, saveCharacter, gameStart, getDialog, getOption, selectOption, showNextDialogBtn, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene, getHistory }
}


const { achieveEnding, getThemesong, playPauseSong, saveCharacter, gameStart, getDialog, getOption, selectOption, showNextDialogBtn, getCurrentState, getEndScene, getName, goHomePage, getCharecterMood, getBackground, showDirectorScene, getHistory } = game()
</script>
<template>
  <!-- Icon Web + Song ------------------------------------------------------------------------------------------------------------------------------->
  <audio ref="effect" />
  <div class="shadow-4xl bg-white pb-4 pr-2 rounded-br-[30px] absolute z-10 w-24 rounded-bl-[10px] rounded-tr-[10px]">
    <img src="./assets/images/element/Logo.png" class="scale-100" />
    <audio ref="inputMusic" id="startMusic-001" autoplay />
    <button fleid="mybtn" class="w-20 h-10 rounded-full hover:scale-[115%] duration-300 each-in-out bg-pink-500 m-1"
      @click="playPauseSong(), effectSound()">
      <span class="flex justify-center text-white">{{ isPlaying ? "Pause" : "Play" }}</span>
    </button>
    <!-- Setting Button -->

  </div>

  <!-- Setting sound v-show setting-->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50" v-show="setting">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl show-dialog">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Sound Settings</div>
      <div class="bg-slate-50 rounded-lg p-5 my-3 delay-500">
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
          <div class="text-lg font-bold"> Change Background Song </div>

        </div>
        <div class="flex">
          <select id="bg-song" v-model="selectsong"
            class="bg-gray-200 border mt-3 border-gray-300 text-lg font-semibold rounded-lg text-slate-700  focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 ">
            <option disabled value="0" selected>Choose a background song...</option>
            <option value="1" class="mx-5">Default</option>
            <option value="2">Marry Me</option>
            <option value="3">Ghostrifter</option>
            <option value="4">Spring</option>
            <option value="5">Matsuri</option>
            <option value="6">Attack on Titan 1</option>
            <option value="7">Attack on Titan 2</option>
            <option value="8">Kick Back</option>
          </select>

          <div
            class="text-center text-xl rounded-md ease-in-out duration-300 mt-3 ml-5 pt-3 px-6  bg-rose-500  text-white font-bold hover:bg-[#f82b74] active:bg-black"
            @click="saveBackgroundsong">
            Save
          </div>
        </div>
      </div>
      <div
        class="shadow-xl rounded-full p-5 mt-8 hover:scale-105 ease-in-out duration-300 bg-rose-500  text-white font-bold hover:bg-[#f82b74] "
        @click="setting = !setting, effectSound()">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>

  <!-- Achievement v-show -->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50 " v-show="achieve">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl show-dialog">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Achievement</div>
      <div class="flex mt-10 overflow">
        <div class="flex flex-col first-letter w-64 show-option delay-500 mx-2">
          <div class="bg-white rounded-xl shadow-gray-400 h-52 pt-4 shadow-xl overflow-hidden"><img
              src="images/character/kaitoon/base.png" class="scale-[150%]"
              :class="achieveEnding()[0] == 0 ? 'brightness-[0%]' : ''"></div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            {{ achieveEnding()[0] == 0 ? 'ENDING 1' : achieveEnding()[0] }} </div>
        </div>
        <div class="flex flex-col first-letter w-64 show-option delay-500 mx-2">
          <div class="bg-white rounded-xl shadow-gray-400 h-52 pt-4 shadow-xl overflow-hidden"><img
              src="images/character/kaitoon/base.png" class="scale-[150%]"
              :class="achieveEnding()[1] == 1 ? 'brightness-[0%]' : ''"></div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            ENDING 2 </div>
        </div>
        <div class="flex flex-col first-letter w-64 show-option delay-500 mx-2">
          <div class="bg-white rounded-xl shadow-gray-400 h-52 pt-4 shadow-xl overflow-hidden"><img
              src="images/character/kaitoon/smile.png" class="scale-[150%]"
              :class="achieveEnding()[2] == 2 ? 'brightness-[0%]' : ''"></div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            ENDING 3 </div>
        </div>
        <div class="flex flex-col first-letter w-64 show-option delay-500 mx-2">
          <div class="bg-white rounded-xl shadow-gray-400 h-52 pt-4 shadow-xl overflow-hidden"><img
              src="images/character/kaitoon/smile.png" class="scale-[150%]"
              :class="achieveEnding()[3] == 3 ? 'brightness-[0%]' : ''"></div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            ENDING 4 </div>
        </div>
        <div class="flex flex-col first-letter w-64 show-option delay-500 mx-2">
          <div class="bg-white rounded-xl shadow-gray-400 h-52 pt-4 shadow-xl overflow-hidden"><img
              src="images/character/kaitoon/sad.png" class="scale-[150%]"
              :class="achieveEnding()[4] == 4 ? 'brightness-[0%]' : ''"></div>
          <div
            class="bg-slate-50 bg-opacity-50 p-3 rounded-xl mt-5 text-center font-bold text-lg shadow-gray-400 shadow-xl">
            ENDING 5 </div>
        </div>



      </div>
      <div
        class=" bg-rose-500  text-white font-bold hover:bg-[#f82b74] rounded-full p-5 mt-20 hover:scale-105 ease-in-out duration-300"
        @click="achieve = !achieve, effectSound()">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>


  <!-- History v-show -->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50 " v-show="history">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl show-dialog">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">History</div>
      <div class="overflow-y-auto flex flex-col mt-5 ">
        <div class=" bg-white rounded-xl shadow-gray-400 shadow-md indent-16 px-3 py-3 my-3"
          v-for="(item, index) in getHistory()"> Question{{ index+1 }} : {{ item }}
        </div>

      </div>

      <div
        class=" bg-rose-500 text-white font-bold hover:bg-[#f82b74] rounded-full p-5 mt-16 hover:scale-105 ease-in-out duration-300"
        @click="history = !history, effectSound()">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>

  <!-- info -->
  <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50 " v-show="info">
    <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl show-dialog">
      <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Game Information</div>
     
      <div class="overflow-y-auto flex flex-col mt-3 ">
        <div class="bg-white rounded-xl px-5 py-5 my-2">
          <div class="text-2xl font-bold px-3">Information</div>
          <hr>
          <div class="text-md indent-10 mt-4 mali">เกมส์นี้เป็นส่วนหนึ่งของวิชา INT203
            CLIENT-SIDE WEB PROGRAMMING II ปีการศึกษา 2565 คณะเทคโนโลยีสารสนเทศ มหาวิทยาลัยเทคโนโลยีพระจอมเกล้าธนบุรี</div>
        </div>
        <div class="bg-white rounded-xl px-5 py-5 my-2">
          <div class="text-2xl font-bold px-3">Browser support</div>
          <hr>
          <div class="text-md indent-10 mt-4 mali">
          เพื่อให้ได้รับประสบการณ์ที่ดีที่สุดในการใช้งาน กรุณาใช้งานผ่าน Browser ที่รองรับ ดังนี้
            <li>Edge เวอร์ชัน 110 ขึ้นไป </li>
            <li>Google Chrome เวอร์ชัน 110 ขึ้นไป </li>
            <li>สัดส่วนหน้าจอ 16:9 </li>
          </div>
        </div>
         <div class="bg-white rounded-xl px-5 py-5 my-2">
          <div class="text-2xl font-bold px-3">Game Support</div>
          <hr>
          <div class="text-md indent-10 mt-4 mali">หากพบปัญหาในการใช้งาน หรือต้องการติดต่อผู้พัฒนา สามารถติดต่อได้ทาง <a href="mailto:project203@kasidate.me?subject=BOI-046%20Support" class="text-blue-500">project203@kasidate.me</a></div>
        </div>

      </div>

      <div
        class=" bg-rose-500  text-white font-bold hover:bg-[#f82b74] rounded-full p-5 mt-16 hover:scale-105 ease-in-out duration-300"
        @click="info = !info, effectSound()">
        <div class="text-center text-2xl font-semibold ">
          CLOSE
        </div>
      </div>
    </div>
  </div>

  <!-- firstpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 1">



    <img src="./assets/images/other/Enerd.png"
      class="absolute -z-20 h-[90%] left-[-10%] scale-[90%] -bottom-16 contrast-120 shadowcharecter delay-900 duration-500 ease-in-out" />
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


<!-- Select Character -->
    <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50" v-show="selectCharacter">
      <div
        class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl overflow-hidden show-dialog">
        <div
          class="text-center color-black bg-white font-extrabold text-4xl rounded-lg p-5 my-3 z-50 shadow-lg bg-opacity-70">
          CHARACTER : <span class="mali mt-5 px-3 text-[#f82b74] ">N' {{ characterinfo[currentCharacter].en.toUpperCase()
          }} </span>
        </div>

        <div class="flex ">
          <div class="flex w-20 h-10 mt-40 text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
            @click="backCharacter"> <ion-icon name="chevron-back-outline"></ion-icon></div>
          <div class="flex -mt-20 ">
            <img :src="characterinfo[currentCharacter].image" class="w-fit" />
          </div>
          <div class="flex w-20 h-10  mt-40  hover:skip text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
            @click="nextCharacter"><ion-icon name="chevron-forward-outline"></ion-icon></div>
        </div>
        <div
          class="bottom-56 -left-[16rem] absolute bg-white text-3xl ml-[47rem] z-50 hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer flex px-8 py-2 mr-5 h-fit justify-center rounded-full border-[#f82b74] border-4 border-solid"
          @click="selectCharacter = !selectCharacter, effectSound()">
          Cancel
        </div>
        <div
          class="bottom-56 left-[30rem] absolute bg-white text-3xl ml-[47rem] z-50 hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer flex px-8 py-2 mr-5 h-fit justify-center rounded-full border-[#f82b74] border-4 border-solid"
          @click="saveCharacter">
          Save
        </div>


      </div>
    </div>


    <div class="w-full h-full relative">

      <!-- help -->
          <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-50" v-show="help">
        <div
          class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl overflow-hidden show-dialog">

          <div class="flex ">
            <div class="flex w-20 h-10 mt-40 text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
              @click="backHowto()"> <ion-icon name="chevron-back-outline"></ion-icon></div>
            <div class="flex -mt-20 ">
              <img :src="Howtoimage[currentHowto].image" class="w-fit" />
            </div>
            <div class="flex w-20 h-10  mt-40  hover:skip text-7xl text-[#f82b74] hover:scale-110 duration-200 ease-in-out"
              @click="nextHowto()"><ion-icon name="chevron-forward-outline"></ion-icon></div>
          </div>
          <div
            class="bottom-56 left-[30rem] absolute bg-white text-3xl ml-[47rem] z-50 hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer flex px-8 py-2 mr-5 h-fit justify-center rounded-full border-[#f82b74] border-4 border-solid"
            @click="showHelp()">
            Close
          </div>

        
        </div>
      </div>
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

          <div @click="selectCharacter = !selectCharacter, effectSound()" 
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
          <div @click="setting = !setting, effectSound()"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-8 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            SETTING
            <div class="pl-4 flex items-center">
              <ion-icon name="settings-sharp"></ion-icon>
            </div>
          </div>
          <div @click="achieve = !achieve, effectSound()"
            class="mali hover:scale-[110%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer mt-8 border-[#f82b74] border-4 border-solid h-[12%] w-fit pl-20 pr-20 ml-64 rounded-full flex justify-center items-center text-4xl bg-white">
            ACHIEVEMENT
            <div class="pl-4 flex items-center">
              <ion-icon name="ribbon-sharp"></ion-icon>
            </div>
          </div>
          <div class="mt-5 h-1/6 w-96 ml-64 grid grid-cols-3 gap-20 p-4">
            <div @click="showHelp"
              class="text-4xl bg-white rounded-full border-[#f82b74] border-4 border-solid flex justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="help-sharp"></ion-icon>
            </div>
            <div @click="info = !info, effectSound()"
              class="text-4xl bg-white rounded-full flex border-[#f82b74] border-4 border-solid justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="information-sharp"></ion-icon>
            </div>
            <a href="https://www.facebook.com/sharer/sharer.php?u=https://game.kasp.codes&t=Let play now!!" @click="effectSound()"
              target="_blank"
              class="text-4xl bg-white rounded-full border-[#f82b74] border-4 border-solid flex justify-center items-center hover:scale-[115%] duration-200 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer">
              <ion-icon name="share-social-sharp"></ion-icon>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- subtitles------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 2">
    <div class="flex w-full h-full bg-red-200 bg-opacity-50 absolute justify-center z-40 " v-show="menu">
      <div class="flex flex-col w-1/2 bg-slate-200 m-48 mt-24 p-16 z-30 bg-opacity-90 rounded-3xl show-dialog">
        <div class="text-center color-black font-extrabold text-4xl rounded-lg p-5 my-3">Game Menu</div>
      
        <div class=" flex flex-col  ">
          <div class="rounded-xl px-5 py-5">
            <div @click="historyMenu"
              class="text-3xl mali font-semibold text-[#f82b74] hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid text-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
              HISTORY
            </div>
            <div @click="goHomePage"
              class="text-3xl mali font-semibold text-[#f82b74] mt-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid text-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
              RESTART
            </div>
            <div @click="settingMenu"
              class="text-3xl mali font-semibold text-[#f82b74] mt-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid text-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
              SOUNDSETTING
            </div>
          </div>

        </div>

        <div
          class=" bg-rose-500  text-white font-bold hover:bg-[#f82b74] rounded-full p-5 mt-10 hover:scale-105 ease-in-out duration-300"
          @click="menu = !menu, effectSound()">
          <div class="text-center text-2xl font-semibold ">
            CLOSE
          </div>
        </div>
      </div>
    </div>

    <div class="w-full h-24 flex absolute">
      <div class="w-30 w-full flex justify-end">
        <!-- Back Btn -->
        <div
          class="z-50  ml-36 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          Scene: {{ count }}
        </div>
        <div @click="menu = !menu, effectSound()"
          class="focus:bg-rose-600 z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white">
          MENU
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
          <div v-show="true" class="z-40 bounce absolute cursor-pointer text-2xl w-20 h-20 bottom-4 right-8">
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
      <div @click="history = !history, effectSound()"
        class="absolute bounce z-30 bottom-4 right-48 text-3xl mali font-semibold text-[#f82b74] cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full p-1 bg-fuchsia-50 retry-btn">
        <div class="bg-[#ffffff] rounded-full py-3 pl-8 pr-8 ">show history</div>
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


