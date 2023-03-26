<script setup>
import BlockContainer from './BlockContainer.vue';
</script>

<template>
  <BlockContainer>
    <div
        class="timer"
        :class="{'timer_active': timerState === $options.TimerState.Playing}"
    >
      <div class="timer__time">
          <p>{{ formattedTime }}</p>
      </div>
      <div class="timer__divider" />
      <div class="timer__actions actions">
        <img
            v-if="timerState === $options.TimerState.Stopped"
            src="src/assets/play.svg"
            alt="play"
            class="actions__button"
            @click="$emit('play', timerId)"
        />
        <img
            v-else
            src="src/assets/pause.svg"
            alt="pause"
            class="actions__button"
            @click="$emit('pause', timerId)"
        />
        <img
            :src="stopActionIconSrc"
            alt="stop"
            class="actions__button"
            @click="$emit('reset', timerId)"
        />
      </div>
    </div>
  </BlockContainer>
</template>

<script>
import { TimerState } from "../TimerState";

export default {
  TimerState,
  props: {
    timerId: {
      type: Number,
      default: null,
    },
    timerState: {
      type: Number,
      default: 0,
    },
    timerValue: {
      type: Number,
      default: null,
    }
  },
  computed: {
    stopActionIconSrc() {
      return this.timerState === TimerState.Playing ? 'src/assets/stop.svg' : 'src/assets/stop-gray.svg';
    },
    formattedTime() {
      let seconds = this.timerValue;

      const hours = Math.floor(seconds / 3600);
      seconds -= hours * 3600;

      let minutes = Math.floor(seconds / 60);
      seconds -= minutes * 60;

      seconds = ("0" + seconds).slice(-2);
      minutes = ("0" + minutes).slice(-2);

      let res = seconds;
      if (minutes > 0 || hours > 0) {
        res = `${minutes}:`+ res;
      }
      if (hours > 0) {
        res = `${hours}:` + res;
      }
      return res;
    }
  }
}
</script>

<style scoped>
.timer {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  color: var(--color-gray);
}
.timer_active {
  color: var(--color-white);
}
.timer_active > .timer__divider {
  background: var(--color-white);
}


.timer__time {
  flex-grow: 1;
  max-height: 49%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.timer__actions {
  flex-grow: 1;
  max-height: 49%;
  display: flex;
  align-items: center;
  gap: 48px;
}
.timer__divider {
  width: 100%;
  height: 1px;
  background: var(--color-gray);
}

.actions {
  justify-content: center;
  align-items: center;
}
.actions__button {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>
