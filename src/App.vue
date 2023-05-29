<template>
  <div class="wrapper w-full min-h-screen bg-[#F2FFF5]">
    <div class="container grid place-items-center h-screen mx-auto">
      <div class="timer">
        <div class="status"><img src="./assets/cofe.svg" alt="" /> STATUS</div>
        <div class="numbers">
          <h1>04</h1>
          <h1>59</h1>
        </div>
        <div class="controls flex gap-5 text-4xl">
          <button class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]">
            <i class="bx bx-dots-horizontal-rounded"></i>
          </button>
          <button class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]">
            <i class="bx bx-play"></i>
          </button>
          <button class="p-6 bg-[rgba(77,218,110,0.15)] rounded-[24px]">
            <i class="bx bx-skip-next"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup></script>

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
  color: #14401d;
}

.status {
  width: 295px;
  height: 48px;
  border: 2px solid #14401d;
  border-radius: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  background-color: rgba(77, 218, 110, 0.15);
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
  background-color: rgba(77, 218, 110, 0.62);
}

.controls button:hover {
  background-color: #86e299;
}
</style>
