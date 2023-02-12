<script setup>
import { computed, ref } from "vue";
import data from "./assets/data/tsundere.json";
import summalize from "./assets/data/summalize.json";
// user input data
let playername = ref("ไกซ์");

//game function :: clouser
function game() {
  let user = { name: "" };
  let score = 0;
  let now = ref({
    dialog:
      "ขออภัย คุณไม่มีสิทธิในการเข้าถึงหน้านี้ หากคิดว่าการแจ้งเตือนนี้ผิดพลาดขอให้ refresh page อีกครั้ง",
  });
  let nextDialogBtn = false;
  let state = ref(1);
  let characterName = { th: "ไข่ต้ม", en: "kaitom" };
  let characterMood = "";
  let imageBackground = "error.png";
  let endData = {};
  let directorScene = false;

  function setPlayer(name) {
    user.name = name;
  }

  function gameStart() {
    setPlayer(playername.value);
    characterMood = "no.png";
    setScene(1);
    state.value = 2;
    score = 0;
  }

  function interactiveDialogs(dialog) {
    let replaceDialog = dialog;
    do {
      if (dialog == undefined) return undefined;
      if (replaceDialog?.includes("$NAME"))
        replaceDialog = replaceDialog.replace("$NAME", user.name);
      if (replaceDialog?.includes("$CHARNAME"))
        replaceDialog = replaceDialog.replace("$CHARNAME", characterName.th);
    } while (replaceDialog.includes("$"));
    return replaceDialog;
  }

  function getDialog() {
    return interactiveDialogs(now.value?.dialog);
  }

  function endScene(type) {
    state.value = 3;
    let keyEnding = "";
    if (type === 0) {
      if (score > 34) {
        keyEnding = "b1";
      } else if (score > 14) {
        keyEnding = "b2";
      } else {
        keyEnding = "b3";
      }
    } else if (type === 9) {
      if (score > 5) {
        keyEnding = "a1";
      } else {
        keyEnding = "a2";
      }
    }
    endData = summalize.find((e) => e.id == keyEnding);
    endData.message = interactiveDialogs(endData.message);
    endData.score = score;
    return getEndScene();
  }

  function getEndScene() {
    return endData;
  }

  function getOption() {
    let options = [];
    if (now.value?.options == undefined) return [];
    now.value.options.forEach((e) => {
      e.message = interactiveDialogs(e.message);
      options.push(e);
    });
    return options;
  }

  function setScene(no) {
    // check to endScene
    if (no === 0) {
      if (now.value.no == 150) endScene(9);
    }

    // set new scene
    const newScene = data.find((e) => e.no == no);
    if (newScene === undefined) {
      now.value = { dialog: "Error" };
      imageBackground = "error.png";
    } else {
      now.value = newScene;
      imageBackground = newScene.background;
      directorScene = newScene.who == "director" ? true : false;
    }
  }
  function showDirectorScene() {
    return directorScene;
  }

  function getCurrentState() {
    return state.value;
  }

  function showNextDialog() {
    nextDialogBtn = getOption().length == 1 ? true : false;
    return nextDialogBtn;
  }

  function selectOption(id) {
    const selected = now.value?.options.find((e) => e.id == id);
    score += selected?.score ?? 0;
    setScene(selected.next);
    if (selected?.characterMood !== null)
      characterMood = selected?.characterMood;
  }

  function getName() {
    return now.value?.who == "user"
      ? user.name
      : now.value?.who == "system"
      ? characterName.th
      : now.value?.who;
  }

  function getCharecterMood() {
    return `images/character/${characterName.en}/${characterMood}`;
  }

  function getBackground() {
    return `images/background/${imageBackground}`;
  }

  function goHomePage() {
    state.value = 1;
  }

  return {
    gameStart,
    getDialog,
    getOption,
    selectOption,
    showNextDialog,
    getCurrentState,
    getEndScene,
    getName,
    goHomePage,
    getCharecterMood,
    getBackground,
    showDirectorScene,
  };
}

const {
  gameStart,
  getDialog,
  getOption,
  selectOption,
  showNextDialog,
  getCurrentState,
  getEndScene,
  getName,
  goHomePage,
  getCharecterMood,
  getBackground,
  showDirectorScene,
} = game();

</script>
<template>
  <div
    class="shadow-4xl bg-white pb-4 pr-2 rounded-br-[30px] absolute z-10 w-24 rounded-bl-[10px] rounded-tr-[10px]"
  >
    <img src="./assets/images/element/Logo.png" class="scale-100" />
  </div>
  <!-- firstpage------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-if="getCurrentState() == 1">
    <img
      src="./assets/images/other/Enerd.png"
      class="absolute -z-20 h-[90%] left-[-10%] scale-[90%] -bottom-16 contrast-150 shadowcharecter delay-900 duration-500 ease-in-out"
    />
    <img
      src="./assets/images/other/EyenCha.png"
      class="absolute -z-40 h-[70%] left-[7%] -top-[52px] scale-[115%] contrast-150 shadowcharecter duration-500"
    />
    <img
      src="./assets/images/other/Enu.png"
      class="absolute -z-20 h-[90%] left-[15%] bottom-14 contrast-[125%] shadowcharecter"
    />
    <img
      src="./assets/images/other/Edu.png"
      class="absolute -z-10 h-[90%] -bottom-32 -left-[-17%] contrast-[125%] left shadowcharecter"
    />
    <img
      src="./assets/images/other/Ejaidee.png"
      class="absolute -z-30 h-[80%] bottom-52 -left-[12%] contrast-100 left shadowcharecter"
    />
    <img
      src="./assets/images/other/Ekaitoon.png"
      class="absolute z-0 h-[120%] -bottom-52 left-[-5%] contrast-[100%] left"
    />
    <img
      src="./assets/images/element/gameName.png"
      class="w-full h-full absolute -z-50"
    />
    <!-- snow animation -->
    <div class="snow-container">
      <div class="snow foreground"></div>
      <div class="snow foreground layered"></div>
      <div class="snow middleground"></div>
      <div class="snow middleground layered"></div>
      <div class="snow background"></div>
      <div class="snow background layered"></div>
    </div>

    <div class="w-full h-full relative">
      <!-- content right -->
      <div class="w-30 w-1/12"></div>
      <!-- <img src="./assets/images/element/jingjung.png"
        class="scale-150 -top-8 absolute right-48 cursor-pointer" /> -->
      <div class="w-1/2 h-2/3 absolute right-0 bottom-12">
        <div class="w-full h-full flex flex-col">
          <div
            class="h-1/6 w-1/2 ml-52 rounded-full flex justify-center items-center text-4xl mali mt-52 text-[#9B4F5E] font-bold"
          >
            <input
              placeholder="Enter Your Name"
              v-model="playername"
              maxlength="18"
              class="text-center boi-input focus:border-[#9B4F5E] rounded-tl-3xl rounded-br-3xl h-24"
            />
          </div>
          <div
            @click="gameStart()"
            class="mali hover:scale-[115%] duration-300 each-in-out text-[#f82b74] font-bold hover:bg-[#f82b74] hover:text-white transition delay-100 hover:border-white cursor-pointer m-16 border-[#f82b74] border-8 border-solid h-1/5 w-96 ml-64 rounded-full flex justify-center items-center text-5xl bg-white"
          >
            PLAY NOW!!!
          </div>
        </div>
      </div>
      <div
        class="w-full h-8 pr-4 bottom-0 absolute flex place-items-center justify-end"
      >
        <p class="copyright">
          © Created By KMUTT Student For Subject INT203 Client-Side Programming
          II
        </p>
      </div>
    </div>
  </div>
  <!-- subtitles------------------------------------------------------------------------------------------------------------------------------->
  <div class="w-screen h-screen" v-show="getCurrentState() == 2">
    <!-- cutScene -->
    <div v-show="showDirectorScene()" class="w-full h-full page-change">
      <img :src="getBackground()" class="absolute w-full h-full -z-50" />
      <div
        class="w-full h-full flex justify-center text-xl font-semibold mali items-center"
        @click="selectOption(getOption()[0].id)"
      >
        <div
          class="relative w-[50%] h-[95%] border-[#f82b74] border-4 flex mr-auto ml-auto indent-10 p-20 items-center bg-white bg-opacity-70 rounded-bl-[100px] rounded-tr-[100px] show-dialog"
        >
          <p class="pb-32 leading-10 text-[#f82b74] typing break-all">
            {{ getDialog() }}
          </p>
          <div
            v-show="true"
            class="z-50 bounce absolute cursor-pointer text-2xl w-20 h-20 bottom-4 right-8"
          >
            <img src="./assets/images/element/skipwhite.png" />
          </div>
        </div>
      </div>
    </div>

    <!-- gameplaypage--------------->
    <div class="w-screen h-screen page-change" v-show="!showDirectorScene()">
      <!-- background image -->
      <img
        :src="getBackground()"
        class="absolute -z-50 w-full h-full justify-center"
      />
      <!-- header bar -->
      <div class="w-full h-full translation-fade-1">
        <div class="w-full h-24 flex flex-row">
          <div class="w-30 w-full flex justify-end">
            <!-- Back Btn -->
            <div
              @click="goHomePage()"
              class="z-50 text-3xl mali font-semibold text-[#f82b74] mt-4 mr-12 hover:bg-rose-600 hover:text-white cursor-pointer border-[#f82b74] border-2 border-solid place-self-center flex place-items-center rounded-[15px] py-3 pl-8 pr-8 bg-white"
            >
              ไออ่อนย้อนเวลา
            </div>
          </div>
        </div>
        <!-- game content -->
        <div class="w-full h-2/3 flex">
          <!-- left + charecter-->
          <div class="w-full flex justify-center relative left -z-50">
            <!-- image -->
            <div class="w-full -top-20 flex justify-center absolute"></div>
            <img :src="getCharecterMood()" class="scale-[175%] -z-50 pr-28" />
          </div>
          <!-- right + option -->
          <div
            class="w-8/12 grid text-[#f82b74] font-semibold text-3xl mali pt-20 pb-20 show-option"
            v-if="!showNextDialog()"
          >
            <!-- choice -->
            <div
              v-for="(choice, index) in getOption()"
              :key="index"
              @click="selectOption(choice.id)"
              v-show="choice.id !== null"
              class="opacity-80 hover:opacity-100 mr-12 break-all w-fit hover:bg-[#f82b74] hover:border-white hover:scale-[105%] duration-300 each-in-out hover:text-white cursor-pointer border-[#f82b74] border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-12 pr-12 bg-fuchsia-50"
            >
              {{ choice.message }}
            </div>
          </div>
        </div>
        <!-- footer -->
        <div
          class="w-full h-56 flex justify-center text-white mali text-2xl font-semibold bg-rose-400 bg-opacity-80 border-t-4 border-white rounded-tl-[20px] show-dialog"
        >
          <div class="w-10/12 relative">
            <!-- dialog -->
            <p class="ml-20 mt-12 mr-28 pt-5">{{ getDialog() }}</p>
            <!-- name -->
            <div
              class="cursor-pointer border-white border-4 -top-9 h-16 absolute ml-28 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-500 rounded-3xl drop-shadow-3xl"
            >
              <p class="flex text-center">{{ getName() }}</p>
            </div>
            <!-- next dialog btn -->
            <div
              v-show="showNextDialog()"
              @click="selectOption(getOption()[0].id)"
              class="bounce cursor-pointer text-2xl w-20 h-20 m-1 absolute bottom-0 right-16 mali flex place-items-center justify-center text-white skip"
            >
              <img src="./assets/images/element/skipwhite.png" />
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
      <div class="w-1/2 border-red-300 border-2 h-[90%]">
        <div
          class="w-5/6 h-1/2 border-red-300 border-2 mt-24 ml-auto mr-auto m-1"
        >
          <!-- Ending Image Character -->
          <img :src="getEndScene().image" />
        </div>
        <div
          class="w-5/6 h-1/3 border-red-300 border-2 mt-4 ml-auto mr-auto m-1"
        >
          <!-- Ending -->
          <img src="" />
        </div>
      </div>
      <!-- right -->
      <div class="w-1/2 ml-1 h-[90%] border-red-300 border-2">
        <!-- score -->
        <div
          class="w-3/4 h-[30%] m-1 mt-24 mr-auto ml-auto border-red-300 border-2"
        >
          {{ getEndScene().score }}
        </div>
        <!-- ending text -->
        <div
          class="w-3/4 h-[45%] mt-16 mr-auto ml-auto border-red-300 border-2 relative flex justify-center"
        >
          <div
            class="mali border-red-300 border-2 border-solid -top-8 h-16 absolute -left-10 flex justify-center place-items-center pl-6 pr-6 text-3xl bg-rose-300 rounded-3xl drop-shadow-3xl"
          >
            <p class="flex text-center">{{ getName() }}</p>
          </div>
          <p class="mr-10 ml-10 mt-12 text-lg mali indent-10">
            {{ getEndScene().message }}
          </p>
        </div>
      </div>
      <div
        @click="goHomePage()"
        class="absolute bottom-4 right-4 text-3xl mali font-semibold text-[#f82b74] hover:bg-rose-600 hover:text-white cursor-pointer border-rose-500 border-y-4 border-solid place-self-center flex place-items-center rounded-full py-3 pl-8 pr-8 bg-fuchsia-50"
      >
        Retry
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
  margin: 8px 0;
  box-sizing: border-box;
  border: 5px solid #f82b74;
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
  0% {
    transition-delay: 0.2s;
    background-color: #b10239;
    opacity: 0.6;
  }

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
    transform: translateX(10%);
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
    transform: translateX(0);
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
    transform: translateX(0);
  }
}

/* snow homepage */
.snow-container {
  position: absolute;
  height: 100%;
  width: 100%;
  max-width: 100%;
  top: 0;
  overflow: hidden;
  z-index: 2;
  pointer-events: none;
}

.snow {
  display: block;
  position: absolute;
  z-index: 2;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  pointer-events: none;
  opacity: 0.5;
  transform: translate3d(0, -100%, 0);
  -webkit-animation: snow linear infinite;
  animation: snow linear infinite;
}

.snow.foreground {
  background-image: url("https://dl6rt3mwcjzxg.cloudfront.net/assets/snow/snow-large-075d267ecbc42e3564c8ed43516dd557.png");
  -webkit-animation-duration: 15s;
  animation-duration: 15s;
}

.snow.foreground.layered {
  -webkit-animation-delay: 7.5s;
  animation-delay: 7.5s;
}

.snow.middleground {
  background-image: image-url(
    "https://dl6rt3mwcjzxg.cloudfront.net/assets/snow/snow-medium-0b8a5e0732315b68e1f54185be7a1ad9.png"
  );
  -webkit-animation-duration: 20s;
  animation-duration: 20s;
}

.snow.middleground.layered {
  -webkit-animation-delay: 10s;
  animation-delay: 10s;
}

.snow.background.layered {
  -webkit-animation-delay: 15s;
  animation-delay: 15s;
}

@keyframes snow {
  0% {
    transform: translate3d(0, -100%, 0);
  }

  100% {
    transform: translate3d(15%, 100%, 0);
  }
}
</style>
