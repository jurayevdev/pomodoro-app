<template>
  <div :class="dark ? classDarkBody : classBody">
    <audio
      :src="soundFocus"
      controls
      class="opacity-0 w-0 h-0 overflow-hidden"
      id="notSoundFocus"
    ></audio>
    <audio
      :src="soundBreak"
      class="opacity-0 w-0 h-0 overflow-hidden"
      id="notSoundBreak"
    ></audio>
    <div class="container grid place-items-center h-screen mx-auto">
      <button @click="toggleModal" :class="dark ? classDarkSetting : classSetting">
        <i :class="modal ? 'bx bx-x' : 'bx bx-cog'"></i>
      </button>
      <div id="dropdown" :class="modal ? classModal : 'hidden'">
        <div class="flex items-start justify-between p-4">
          <h3 class="text-xl font-semibold">Settings</h3>
        </div>
        <div class="p-4 flex items-center justify-between">
          Dark mode
          <el-switch v-model="dark" />
        </div>
        <div class="p-4 flex items-center justify-between">
          Focus length
          <input
            @click="subFocus"
            v-model="inputNumber.focus"
            type="number"
            class="w-20 rounded-lg"
          />
        </div>
        <div class="p-4 flex items-center justify-between">
          Short break length
          <input
            @click="subSHbreak"
            v-model="inputNumber.shBreak"
            type="number"
            class="w-20 rounded-lg"
          />
        </div>
        <div class="p-4 flex items-center justify-between">
          Long break length
          <input
            @click="subLbreak"
            v-model="inputNumber.lBreak"
            type="number"
            class="w-20 rounded-lg"
          />
        </div>
        <div class="p-4 flex items-center justify-between">
          Auto resume timer
          <el-switch v-model="autoResume" />
        </div>
        <div class="p-4 flex items-center justify-between">
          Sound
          <el-switch v-model="sound" />
        </div>
      </div>

      <div class="timer">
        <div :class="dark ? classDarkStatus : classStatus">
          <i :class="work ? 'bx bx-coffee text-[30px]' : 'bx bx-brain text-[30px]'"></i
          >{{ status }}
        </div>
        <div class="numbers">
          <h1 id="min" :class="dark ? classDarkNumber : classNumber">25</h1>
          <h1 id="sekunt" :class="dark ? classDarkNumber : classNumber">00</h1>
        </div>
        <div class="controls flex gap-5 text-4xl">
          <button @click="refresh" :class="dark ? classDarkBtn : classBtn">
            <i class="bx bx-refresh"></i>
          </button>
          <button @click="btn" id="play" :class="dark ? classDarkBtn : classBtn">
            <i :class="!lamp ? 'bx bx-play' : 'bx bx-pause'"></i>
          </button>
          <button @click="next" :class="dark ? classDarkBtn : classBtn">
            <i class="bx bx-skip-next"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, reactive } from "vue";
import soundFocus from "./assets/sound/focus.mp3";
import soundBreak from "./assets/sound/break.mp3";

const modal = ref(false);
const toggleModal = () => (modal.value = !modal.value);

const dark = ref(false);
const autoResume = ref(false);
const sound = ref(true);

const inputNumber = reactive({
  focus: 25,
  shBreak: 5,
  lBreak: 15,
});

const status = ref("FOCUS");
const lamp = ref(false);
const minute = ref(inputNumber.focus - 1);
const end = ref(60);
const setTimeOutResult = ref(null);
const work = ref(false);
const sch = ref(0);

const classSetting = ref("absolute right-4 top-4 text-[30px] text-[#471515]");
const classDarkSetting = ref("absolute right-4 top-4 text-[30px] text-[#FFF2F2]");

const classModal = ref(
  "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#471515]"
);

const classBtn = ref(
  "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]"
);
const classDarkBtn = ref(
  "p-6 bg-[#471515] rounded-[24px] text-[#FFF2F2] hover:bg-[rgba(255,76,76,0.55)] focus:bg-[rgba(255,76,76,0.71)]"
);
const classBody = ref("wrapper w-full min-h-screen bg-[#FFF2F2]");
const classDarkBody = ref("wrapper w-full min-h-screen bg-[#0D0404]");
const classStatus = ref(
  "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]"
);
const classDarkStatus = ref(
  "status border-2 border-[#FFF2F2] text-[#FFF2F2] bg-[#471515]"
);
const classNumber = ref("text-[#471515]");
const classDarkNumber = ref("text-[#FFF2F2]");

// ----------------- submit imput start --------------- //

const playSoundFocus = () => {
  let soundPlayFocus = document.querySelector("#notSoundFocus");
  soundPlayFocus.play();
};

const playSoundBreak = () => {
  let soundPlayBreak = document.querySelector("#notSoundBreak");
  soundPlayBreak.play();
};

let subFocus = () => {
  if (status.value == "FOCUS") {
    clearInterval(setTimeOutResult.value);
    lamp.value = false;
    minute.value = inputNumber.focus - 1;
    if (inputNumber.focus < 10) {
      min.textContent = `0${inputNumber.focus}`;
    } else {
      min.textContent = inputNumber.focus;
    }
    sekunt.textContent = "00";
    end.value = 60;
  }
};

let subSHbreak = () => {
  if (status.value == "SHORT BREAK") {
    clearInterval(setTimeOutResult.value);
    lamp.value = false;
    minute.value = inputNumber.shBreak - 1;
    if (inputNumber.shBreak < 10) {
      min.textContent = `0${inputNumber.shBreak}`;
    } else {
      min.textContent = inputNumber.shBreak;
    }
    sekunt.textContent = "00";
    end.value = 60;
  }
};

let subLbreak = () => {
  if (status.value == "LONG BREAK") {
    clearInterval(setTimeOutResult.value);
    lamp.value = false;
    minute.value = inputNumber.lBreak - 1;
    if (inputNumber.lBreak < 10) {
      min.textContent = `0${inputNumber.lBreak}`;
    } else {
      min.textContent = inputNumber.lBreak;
    }
    sekunt.textContent = "00";
    end.value = 60;
  }
};

// ----------------- submit imput end --------------- //

// ----------------- refref button start --------------- //

let refresh = () => {
  clearInterval(setTimeOutResult.value);
  if (work.value) {
    if (status.value == "LONG BREAK") {
      minute.value = inputNumber.lBreak - 1;
      if (inputNumber.lBreak < 10) {
        min.textContent = `0${minute.value + 1}`;
      } else {
        min.textContent = minute.value + 1;
      }
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
    } else {
      minute.value = inputNumber.shBreak - 1;
      if (inputNumber.shBreak < 10) {
        min.textContent = `0${minute.value + 1}`;
      } else {
        min.textContent = minute.value + 1;
      }
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
    }
  } else {
    minute.value = inputNumber.focus - 1;
    if (inputNumber.focus < 10) {
      min.textContent = `0${minute.value + 1}`;
    } else {
      min.textContent = minute.value + 1;
    }
    end.value = 60;
    sekunt.textContent = "00";
    status.value = "FOCUS";
  }
  lamp.value = false;
};

// ----------------- refref button end --------------- //

// ----------------- next button start --------------- //

let next = () => {
  clearInterval(setTimeOutResult.value);
  if (status.value == "FOCUS") {
    if (sch.value == 3) {
      minute.value = inputNumber.lBreak - 1;
      if (inputNumber.lBreak < 10) {
        min.textContent = `0${minute.value + 1}`;
      } else {
        min.textContent = minute.value + 1;
      }
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
      lamp.value = false;
      sch.value = 0;
      classModal.value =
        "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#153047]";

      classBtn.value =
        "p-6 bg-[rgba(76,172,255,0.15)] rounded-[24px] text-[#153047] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";
      classDarkBtn.value =
        "p-6 bg-[#153047] rounded-[24px] text-[#F2F9FF] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";

      classBody.value = "wrapper w-full min-h-screen bg-[#F2F9FF]";
      classDarkBody.value = "wrapper w-full min-h-screen bg-[#04090D]";

      classStatus.value =
        "status border-2 border-[#153047] text-[#153047] bg-[rgba(76,172,255,0.15)]";
      classDarkStatus.value =
        "status border-2 border-[#F2F9FF] text-[#F2F9FF] bg-[#153047]";

      classNumber.value = "text-[#153047]";
      classDarkNumber.value = "text-[#F2F9FF]";

      classSetting.value = "absolute right-4 top-4 text-[30px] text-[#153047]";
      classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#F2F9FF]";
    } else {
      minute.value = inputNumber.shBreak - 1;
      if (inputNumber.shBreak < 10) {
        min.textContent = `0${minute.value + 1}`;
      } else {
        min.textContent = minute.value + 1;
      }
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
      lamp.value = false;
      sch.value = sch.value + 1;
      classModal.value =
        "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#14401D]";

      classBtn.value =
        "p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]";
      classDarkBtn.value =
        "p-6 bg-[#14401D] rounded-[24px] text-[#F2FFF5] hover:bg-[rgba(77,218,110,0.40)] focus:bg-[rgba(77,218,110,0.62)]";

      classBody.value = "wrapper w-full min-h-screen bg-[#F2FFF5]";
      classDarkBody.value = "wrapper w-full min-h-screen bg-[#040D06]";

      classStatus.value =
        "status border-2 border-[#14401d] text-[#14401d] bg-[rgba(77,218,110,0.15)]";
      classDarkStatus.value =
        "status border-2 border-[#F2FFF5] text-[#F2FFF5] bg-[#14401D]";

      classNumber.value = "text-[#14401d]";
      classDarkNumber.value = "text-[#F2FFF5]";

      classSetting.value = "absolute right-4 top-4 text-[30px] text-[#14401d]";
      classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#F2FFF5]";
    }
  } else {
    minute.value = inputNumber.focus - 1;
    if (inputNumber.focus < 10) {
      min.textContent = `0${minute.value + 1}`;
    } else {
      min.textContent = minute.value + 1;
    }
    end.value = 60;
    sekunt.textContent = "00";
    status.value = "FOCUS";
    lamp.value = false;
    classModal.value =
      "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#471515]";

    classBtn.value =
      "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]";
    classDarkBtn.value =
      "p-6 bg-[#471515] rounded-[24px] text-[#FFF2F2] hover:bg-[rgba(255,76,76,0.55)] focus:bg-[rgba(255,76,76,0.71)]";

    classBody.value = "wrapper w-full min-h-screen bg-[#FFF2F2]";
    classDarkBody.value = "wrapper w-full min-h-screen bg-[#0D0404]";

    classStatus.value =
      "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]";
    classDarkStatus.value =
      "status border-2 border-[#FFF2F2] text-[#FFF2F2] bg-[#471515]";

    classNumber.value = "text-[#471515]";
    classDarkNumber.value = "text-[#FFF2F2]";

    classSetting.value = "absolute right-4 top-4 text-[30px] text-[#471515]";
    classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#FFF2F2]";
  }
  work.value = !work.value;
};

// ----------------- next button end --------------- //

// ----------------- play and pouse button start --------------- //

let btn = () => {
  lamp.value = !lamp.value;

  start();

  if (!lamp.value) {
    clearInterval(setTimeOutResult.value);
  }
};

let start = () => {
  if (lamp.value) {
    setTimeOutResult.value = setInterval(() => {
      end.value = end.value - 1;
      sekunt.textContent = end.value;

      if (minute.value < 10) {
        min.textContent = `0${minute.value}`;
      } else {
        min.textContent = minute.value;
      }

      if (end.value < 10) {
        sekunt.textContent = `0${end.value}`;
      }
      if (end.value == 0 && minute.value !== -1) {
        end.value = 60;
        minute.value = minute.value - 1;
      }

      if (minute.value == -1) {
        stop();
      }
    }, 1000);
  }
};

let stop = () => {
  if (minute.value == -1) {
    if (!autoResume.value){
      clearInterval(setTimeOutResult.value);
    }
    work.value = !work.value;
    if (work.value) {
      if (sch.value == 3) {
        minute.value = inputNumber.lBreak - 1;
        if (inputNumber.lBreak < 10) {
          min.textContent = `0${minute.value + 1}`;
        } else {
          min.textContent = minute.value + 1;
        }
        sch.value = 0;
        status.value = "LONG BREAK";
        if (sound.value) {
          playSoundBreak();
        }
        classModal.value =
          "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#153047]";

        classBtn.value =
          "p-6 bg-[rgba(76,172,255,0.15)] rounded-[24px] text-[#153047] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";
        classDarkBtn.value =
          "p-6 bg-[#153047] rounded-[24px] text-[#F2F9FF] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";

        classBody.value = "wrapper w-full min-h-screen bg-[#F2F9FF]";
        classDarkBody.value = "wrapper w-full min-h-screen bg-[#04090D]";

        classStatus.value =
          "status border-2 border-[#153047] text-[#153047] bg-[rgba(76,172,255,0.15)]";
        classDarkStatus.value =
          "status border-2 border-[#F2F9FF] text-[#F2F9FF] bg-[#153047]";

        classNumber.value = "text-[#153047]";
        classDarkNumber.value = "text-[#F2F9FF]";

        classSetting.value = "absolute right-4 top-4 text-[30px] text-[#153047]";
        classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#F2F9FF]";
      } else {
        minute.value = inputNumber.shBreak - 1;
        if (inputNumber.shBreak < 10) {
          min.textContent = `0${minute.value + 1}`;
        } else {
          min.textContent = minute.value + 1;
        }
        sch.value = sch.value + 1;
        status.value = "SHORT BREAK";
        if (sound.value) {
          playSoundBreak();
        }
        classModal.value =
          "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#14401D]";

        classBtn.value =
          "p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]";
        classDarkBtn.value =
          "p-6 bg-[#14401D] rounded-[24px] text-[#F2FFF5] hover:bg-[rgba(77,218,110,0.40)] focus:bg-[rgba(77,218,110,0.62)]";

        classBody.value = "wrapper w-full min-h-screen bg-[#F2FFF5]";
        classDarkBody.value = "wrapper w-full min-h-screen bg-[#040D06]";

        classStatus.value =
          "status border-2 border-[#14401d] text-[#14401d] bg-[rgba(77,218,110,0.15)]";
        classDarkStatus.value =
          "status border-2 border-[#F2FFF5] text-[#F2FFF5] bg-[#14401D]";

        classNumber.value = "text-[#14401d]";
        classDarkNumber.value = "text-[#F2FFF5]";

        classSetting.value = "absolute right-4 top-4 text-[30px] text-[#14401d]";
        classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#F2FFF5]";
      }
    } else {
      minute.value = inputNumber.focus - 1;
      if (inputNumber.focus < 10) {
        min.textContent = `0${minute.value + 1}`;
      } else {
        min.textContent = minute.value + 1;
      }
      status.value = "FOCUS";
      if (sound.value) {
        playSoundFocus();
      }
      classModal.value =
        "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px] text-[#471515]";

      classBtn.value =
        "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]";
      classDarkBtn.value =
        "p-6 bg-[#471515] rounded-[24px] text-[#FFF2F2] hover:bg-[rgba(255,76,76,0.55)] focus:bg-[rgba(255,76,76,0.71)]";

      classBody.value = "wrapper w-full min-h-screen bg-[#FFF2F2]";
      classDarkBody.value = "wrapper w-full min-h-screen bg-[#0D0404]";

      classStatus.value =
        "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]";
      classDarkStatus.value =
        "status border-2 border-[#FFF2F2] text-[#FFF2F2] bg-[#471515]";

      classNumber.value = "text-[#471515]";
      classDarkNumber.value = "text-[#FFF2F2]";

      classSetting.value = "absolute right-4 top-4 text-[30px] text-[#471515]";
      classDarkSetting.value = "absolute right-4 top-4 text-[30px] text-[#FFF2F2]";
    }
    end.value = 60;
    if (!autoResume.value) {
      lamp.value = false;
    }
  }
};

// ----------------- play and pouse button end --------------- //

onMounted(() => {});
</script>

<style lang="scss" scoped>
.timer {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.numbers {
  margin: 32px 0;
}
.numbers h1 {
  font-size: 256px;
  line-height: 217px;
  font-weight: 800;
}

.status {
  width: 200px;
  height: 48px;
  border-radius: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.controls {
  height: 100px;
  display: flex;
  align-items: center;
}
.controls button {
  transition: all 0.3s ease;
}

.controls button:focus {
  padding: 32px 48px;
}
</style>
