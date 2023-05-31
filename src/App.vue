<template>
  <div :class="classBody">
    <div class="container grid place-items-center h-screen mx-auto">
      <button
        @click="toggleModal"
        :class="
          work
            ? 'absolute right-4 top-4 text-[30px] text-[#14401d]'
            : 'absolute right-4 top-4 text-[30px] text-[#471515]'
        "
      >
        <i :class="modal ? 'bx bx-x' : 'bx bx-cog'"></i>
      </button>
      <div id="dropdown" :class="modal ? classModal : 'hidden'">
        <div class="flex items-start justify-between p-4">
          <h3 class="text-xl font-semibold text-gray-900">Settings</h3>
        </div>
        <div class="p-4 flex items-center justify-between">
          Dark mode
          <el-switch v-model="dark" />
        </div>
        <div class="p-4 flex items-center justify-between">
          Focus length
          <input type="number" class="w-20 rounded-lg" value="25" />
        </div>
        <div class="p-4 flex items-center justify-between">
          Short break length
          <input type="number" class="w-20 rounded-lg" value="5" />
        </div>
        <div class="p-4 flex items-center justify-between">
          Long break length
          <input type="number" class="w-20 rounded-lg" value="15" />
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
        <div :class="classStatus">
          <img :src="!work ? miyya : cofe" alt="" />{{ status }}
        </div>
        <div class="numbers">
          <h1 id="min" :class="classNumber">25</h1>
          <h1 id="sekunt" :class="classNumber">00</h1>
        </div>
        <div class="controls flex gap-5 text-4xl">
          <button @click="refresh" :class="classBtn">
            <i class="bx bx-refresh"></i>
          </button>
          <button @click="btn" id="play" :class="classBtn">
            <i :class="!lamp ? 'bx bx-play' : 'bx bx-pause'"></i>
          </button>
          <button @click="next" :class="classBtn">
            <i class="bx bx-skip-next"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import miyya from "./assets/miyya.svg";
import cofe from "./assets/cofe.svg";

const modal = ref(false);
const toggleModal = () => (modal.value = !modal.value);

const status = ref("FOCUS");
const lamp = ref(false);
const minute = ref(24);
const end = ref(60);
const setTimeOutResult = ref(null);
const work = ref(false);
const sch = ref(0);
const classModal = ref(
  "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]"
);
const classBtn = ref(
  "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]"
);
const classBody = ref("wrapper w-full min-h-screen bg-[#FFF2F2]");
const classStatus = ref(
  "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]"
);
const classNumber = ref("text-[#471515]");

// ----------------- refref button start --------------- //

let refresh = () => {
  clearInterval(setTimeOutResult.value);
  if (work.value) {
    if (status.value == "LONG BREAK") {
      minute.value = 14;
      min.textContent = "15";
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
    } else {
      minute.value = 4;
      min.textContent = "05";
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
    }
  } else {
    minute.value = 24;
    min.textContent = "25";
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
      minute.value = 14;
      min.textContent = minute.value + 1;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
      lamp.value = false;
      sch.value = 0;
      classModal.value =
        "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
      classBtn.value =
        "p-6 bg-[rgba(76,172,255,0.15)] rounded-[24px] text-[#153047] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";
      classBody.value = "wrapper w-full min-h-screen bg-[#F2F9FF]";
      classStatus.value =
        "status border-2 border-[#153047] text-[#153047] bg-[rgba(76,172,255,0.15)]";
      classNumber.value = "text-[#153047]";
    } else {
      minute.value = 4;
      min.textContent = `0${minute.value + 1}`;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
      lamp.value = false;
      sch.value = sch.value + 1;
      classModal.value =
        "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
      classBtn.value =
        "p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]";
      classBody.value = "wrapper w-full min-h-screen bg-[#F2FFF5]";
      classStatus.value =
        "status border-2 border-[#14401d] text-[#14401d] bg-[rgba(77,218,110,0.15)]";
      classNumber.value = "text-[#14401d]";
    }
  } else {
    minute.value = 24;
    min.textContent = minute.value + 1;
    end.value = 60;
    sekunt.textContent = "00";
    status.value = "FOCUS";
    lamp.value = false;
    classModal.value =
      "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
    classBtn.value =
      "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]";
    classBody.value = "wrapper w-full min-h-screen bg-[#FFF2F2]";
    classStatus.value =
      "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]";
    classNumber.value = "text-[#471515]";
  }
  work.value = !work.value;
};

// ----------------- next button end --------------- //

// ----------------- play and pouse button start --------------- //

let btn = () => {
  lamp.value = !lamp.value;

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
        clearInterval(setTimeOutResult.value);
        work.value = !work.value;
        if (work.value) {
          if (sch.value == 3) {
            minute.value = 14;
            min.textContent = minute.value + 1;
            sch.value = 0;
            status.value = "LONG BREAK";
            classModal.value =
              "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
            classBtn.value =
              "p-6 bg-[rgba(76,172,255,0.15)] rounded-[24px] text-[#153047] hover:bg-[rgba(76,172,255,0.40)] focus:bg-[rgba(76,172,255,0.62)]";
            classBody.value = "wrapper w-full min-h-screen bg-[#F2F9FF]";
            classStatus.value =
              "status border-2 border-[#14401d] text-[#14401d] bg-[rgba(77,218,110,0.15)]";
            classStatus.value =
              "status border-2 border-[#153047] text-[#153047] bg-[rgba(76,172,255,0.15)]";
            classNumber.value = "text-[#153047]";
          } else {
            minute.value = 4;
            min.textContent = `0${minute.value + 1}`;
            sch.value = sch.value + 1;
            status.value = "SHORT BREAK";
            classModal.value =
              "absolute top-16 bg-[#F2FFF5] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
            classBtn.value =
              "p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]";
            classBody.value = "wrapper w-full min-h-screen bg-[#F2FFF5]";
            classStatus.value =
              "status border-2 border-[#14401d] text-[#14401d] bg-[rgba(77,218,110,0.15)]";
            classNumber.value = "text-[#14401d]";
          }
        } else {
          minute.value = 24;
          min.textContent = minut.value + 1;
          status.value = "FOCUS";
          classModal.value =
            "absolute top-16 bg-[#FFF2F2] right-5 md:top-12 md:right-12 z-10 rounded-[24px] shadow-lg w-[350px]";
          classBtn.value =
            "p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]";
          classBody.value = "wrapper w-full min-h-screen bg-[#FFF2F2]";
          classStatus.value =
            "status border-2 border-[#471515] text-[#471515] bg-[rgba(255,76,76,0.15)]";
          classNumber.value = "text-[#471515]";
        }
        end.value = 60;
        lamp.value = false;
      }
    }, 100);
  }

  if (!lamp.value) {
    clearInterval(setTimeOutResult.value);
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
  gap: 20px;
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
