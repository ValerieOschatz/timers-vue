<template>
  <div class="item">
    <span :class="`item__text ${this.running && 'item__text_running'}`">
      {{ time }}
    </span>
    <div :class="`item__btn-container ${this.running && 'item__btn-container_running'}`">
      <button v-if="running" class="item__button item__button_pause" type="button" @click="stopTimer"></button>
      <button v-else class="item__button item__button_start" type="button" @click="startTimer"></button>
      <button :class="`item__button ${this.running && 'item__button_running'}`" type="button" @click="cancelTimer"></button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
		return {
			sec: 0,
      min: 0,
      hour: 0,
      savedSec: 0,
      running: false,
      interval: null,
		}
	},
  computed: {
    time() {
      return `
        ${this.hour > 0 ? this.hour + ':' : ''}
        ${this.min > 0 ? (this.min < 10 && this.hour > 0 ? '0' + this.min : this.min) + ':' : ''}
        ${(this.sec < 10 && this.min > 0) ? '0' + this.sec : this.sec}
      `;
    }
  },
  methods: {
    startTimer() {
      this.running = true;
      const date = new Date();
      const time = date.getTime();

      this.interval = setInterval(() => {
        const newDate = new Date();
        const newTime = newDate.getTime();

        const t = (newTime - time)/1000 - this.sec  + this.savedSec;

        if (t > 0) {
          this.sec += 1;
        }

        if (this.sec > 59) {
          this.min += 1;
          this.sec = 0;
        }

        if (this.min > 59) {
          this.hour += 1;
          this.min = 0;
        }
      }, 1000)

      this.started = true;
    },
    stopTimer() {
      clearInterval(this.interval);
      this.running = false;
      this.savedSec = this.sec;
    },
    cancelTimer() {
      this.stopTimer();
      this.sec = 0;
      this.min = 0;
      this.hour = 0;
      this.savedSec = 0;
    },
  },
  unmounted() {
    this.stopTimer();
  },
}
</script>
