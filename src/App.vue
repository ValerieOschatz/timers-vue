<script>
import TimerItem from './components/TimerItem.vue';
import './index.css';

export default {
  name: 'App',
  components: {
    TimerItem
  },
  data() {
		return {
			timerList: []
		}
	},
  methods: {
    handleAddTimer() {
      this.timerList = [...this.timerList, {}];
    }
	},
}
</script>

<template>
  <div class="page">
    <ul class="list">
      <li v-for="(timer, index) in timerList" :key="index">
        <TimerItem />
      </li>
      <button class="list__button" type="button" @click="handleAddTimer">+</button>
    </ul>
  </div>
</template>
