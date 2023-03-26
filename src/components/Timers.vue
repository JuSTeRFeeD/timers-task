<script setup>
import TimerItem from '../components/TimerItem.vue';
import BlockContainer from '../components/BlockContainer.vue';
</script>

<template>
  <div class="timers">
    <TimerItem
        v-for="(item, index) in timers"
        :key="index"
        :timer-id="index"
        :timer-value="item.time"
        :timer-state="item.state"
        @play="startTimer"
        @pause="stopTimer"
        @reset="resetTimer"
    />
    <BlockContainer>
      <button class="add-button" @click="addNewTimer">
        <img src="src/assets/plus.svg" alt="+" />
      </button>
    </BlockContainer>
  </div>
</template>

<script>
import { TimerState } from "../TimerState";

export default  {
  data() {
    return {
      timers: Array(0),
    }
  },
  methods: {
    createTimer() {
      return {
        intervalId: null,
        startTime: null,
        time: 0,
        state: TimerState.Stopped,
      };
    },
    addNewTimer() {
      this.timers.push(this.createTimer());
    },
    startTimer(timerId) {
      const timer = this.timers[timerId];
      const { time, state } = timer;

      if (state === TimerState.Playing) return;

      if (time === 0) {
        timer.startTime = Date.now();
      } else {
        timer.startTime = Date.now() - time * 1000;
      }

      timer.intervalId = setInterval(() => {
        const deltaTime = Date.now() - timer.startTime;
        timer.time = Math.floor(deltaTime / 1000);
      }, 1000);

      timer.state = TimerState.Playing;
    },
    stopTimer(timerId) {
      const timer = this.timers[timerId];

      if (timer.intervalId != null) {
        clearInterval(timer.intervalId);
      }

      timer.state = TimerState.Stopped;
    },
    resetTimer(timerId) {
      const timer = this.timers[timerId];

      if (timer.state === TimerState.Playing) {
        clearInterval(timer.intervalId);
      }

      this.timers[timerId] = this.createTimer();
    },
  }
}
</script>

<style scoped>
.timers {
  padding: 72px 49px;
  display: grid;
  justify-content: center;
  gap: 45px 50px;
}

.add-button {
  cursor: pointer;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-width: 320px) {
  .timers {
    grid-template-columns: repeat(1, 225px);
  }
}

@media (min-width: 768px) {
  .timers {
    grid-template-columns: repeat(2, 225px);
  }
}

@media (min-width: 1024px) {
  .timers {
    grid-template-columns: repeat(3, 225px);
  }
}

</style>
