<script>
export default {
  name: 'TimerItem',
  data() {
		return {
			sec: 0,
      min: 0,
      hour: 0,
      running: false,
      interval: null
		}
	},
  methods: {
    startTimer() {
      this.running = true;

      this.interval = setInterval(() => {
        this.sec += 1;

        if (this.sec === 60) {
          this.min += 1;
          this.sec = 0;
        }
        if (this.min === 60) {
          this.hour += 1;
          this.min = 0;
        }
      }, 1000)
    },

    stopTimer() {
      this.running = false;
      clearInterval(this.interval);
    },

    cancelTimer() {
      this.stopTimer();
      this.sec = 0;
      this.min = 0;
      this.hour = 0;
    },
  },

  unmounted() {
    this.stopTimer();
  },
}
</script>

<template>
  <div class="item">
    <span :class="`item__text ${this.running && 'item__text_running'}`">
      {{ 
        `${hour > 0 ? hour + ':' : ''}${min > 0 ? (min < 10 && hour > 0 ? '0' + min : min) + ':' : ''}${(sec < 10 && min > 0) ? '0' + sec : sec}`
      }}
    </span>
    <div :class="`item__btn-container ${this.running && 'item__btn-container_running'}`">
      <button v-if="running" class="item__button item__button_pause" type="button" @click="stopTimer"></button>
      <button v-else class="item__button item__button_start" type="button" @click="startTimer"></button>
      <button :class="`item__button ${this.running && 'item__button_running'}`" type="button" @click="cancelTimer"></button>
    </div>
  </div>
</template>
