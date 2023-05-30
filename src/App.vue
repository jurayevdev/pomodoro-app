<template>
  <div
    :class="
      !work
        ? 'wrapper w-full min-h-screen bg-[#FFF2F2]'
        : 'wrapper w-full min-h-screen bg-[#F2FFF5]'
    "
  >
    <div class="container grid place-items-center h-screen mx-auto">
      <div class="timer">
        <div
          :class="
            work
              ? 'status border-2 border-[#14401d] bg-[rgba(77,218,110,0.15)]'
              : 'status border-2 border-[#471515] bg-[rgba(255,76,76,0.15)]'
          "
        >
          <img :src="!work ? miyya : cofe" alt="" />{{ status }}
        </div>
        <div class="numbers">
          <h1 id="min" :class="work ? 'text-[#14401d]' : 'text-[#471515]'">25</h1>
          <h1 id="sekunt" :class="work ? 'text-[#14401d]' : 'text-[#471515]'">00</h1>
        </div>
        <div class="controls flex gap-5 text-4xl">
          <button
            @click="refresh"
            :class="
              work
                ? 'p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]'
                : 'p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]'
            "
          >
            <i class="bx bx-refresh"></i>
          </button>
          <button
            @click="btn"
            id="play"
            :class="
              work
                ? 'p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]'
                : 'p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]'
            "
          >
            <i :class="!lamp ? 'bx bx-play' : 'bx bx-pause'"></i>
          </button>
          <button
            @click="next"
            :class="
              work
                ? 'p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px] text-[#14401D] hover:bg-[#86e299] focus:bg-[rgba(77,218,110,0.62)]'
                : 'p-6 bg-[rgba(255,76,76,0.15)] rounded-[24px] text-[#471515] hover:bg-[#ee8989] focus:bg-[rgba(255,76,76,0.71)]'
            "
          >
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

const status = ref("FOCUS");
const lamp = ref(false);
const minute = ref(25);
const end = ref(60);
const setTimeOutResult = ref(null);
const work = ref(false);
const sch = ref(0);
const nextLamp = ref(false);

let refresh = () => {
  clearInterval(setTimeOutResult.value);
  if (work.value) {
    if (status.value == "LONG BREAK") {
      minute.value = 15;
      min.textContent = minute.value;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
    } else {
      minute.value = 5;
      min.textContent = `0${minute.value}`;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
    }
  } else {
    minute.value = 25;
    min.textContent = minute.value;
    end.value = 60;
    sekunt.textContent = "00";
    status.value = "FOCUS";
  }
  lamp.value = false;
};

let next = () => {
  nextLamp.value = !nextLamp.value;
  clearInterval(setTimeOutResult.value);
  if (nextLamp.value) {
    if (sch.value == 3) {
      minute.value = 15;
      min.textContent = minute.value;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "LONG BREAK";
      lamp.value = false;
      sch.value = 0;
    } else {
      minute.value = 5;
      min.textContent = `0${minute.value}`;
      end.value = 60;
      sekunt.textContent = "00";
      status.value = "SHORT BREAK";
      lamp.value = false;
      sch.value = sch.value + 1;
    }
  } else {
    minute.value = 25;
    min.textContent = minute.value;
    end.value = 60;
    sekunt.textContent = "00";
    status.value = "FOCUS";
    lamp.value = false;
  }
  work.value = !work.value;
};

let btn = () => {
  lamp.value = !lamp.value;

  if (lamp.value) {
    setTimeOutResult.value = setInterval(() => {
      end.value = end.value - 1;
      sekunt.textContent = end.value;

      if (end.value < 10) {
        sekunt.textContent = `0${end.value}`;
      }
      if (end.value == 0 && minute.value !== -1) {
        end.value = 60;
        minute.value = minute.value - 1;

        if (minute.value < 10) {
          min.textContent = `0${minute.value}`;
        } else {
          min.textContent = minute.value;
        }
      }

      if (minute.value == -1) {
        clearInterval(setTimeOutResult.value);
        work.value = !work.value;
        if (work.value) {
          if (sch.value == 3) {
            minute.value = 15;
            min.textContent = minute.value;
            sch.value = 0;
            status.value = "LONG BREAK";
          } else {
            minute.value = 5;
            min.textContent = `0${minute.value}`;
            sch.value = sch.value + 1;
            status.value = "SHORT BREAK";
          }
        } else {
          minute.value = 25;
          min.textContent = minute.value;
          status.value = "FOCUS";
        }
        end.value = 60;
        lamp.value = false;
      }
    }, 10);
  }

  if (!lamp.value) {
    clearInterval(setTimeOutResult.value);
  }
};

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
  width: 295px;
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
