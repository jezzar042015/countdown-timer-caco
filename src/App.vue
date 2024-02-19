<script>
export default {
  data() {
    return {
      startTimestamp: 0,
      totalTime: 20 * 60, // Total time in seconds
      remainingTime: 20 * 60, // Remaining time in seconds
      run: false,
      minutes: 0,
      seconds: 0
    };
  },
  methods: {
    twoDigitFormat(value) {
      return value < 10 ? `0${value}` : value;
    },
    startTimer() {
      if (!this.run) {
        this.run = true;
        this.startTimestamp = Date.now();

        const intervalId = setInterval(() => {
          const elapsedTime = Math.floor((Date.now() - this.startTimestamp) / 1000);
          this.remainingTime = Math.max(0, this.totalTime - elapsedTime);
          this.minutes = Math.floor(this.remainingTime / 60);
          this.seconds = this.remainingTime % 60;

          if (this.remainingTime <= 0) {
            clearInterval(intervalId);
            this.run = false;
            // Timer reached 0, you can handle any logic here when the timer is complete
          }
        }, 1000); // Update every 1000 milliseconds (1 second)

        // Save the intervalId to be cleared later
        this.$data.intervalId = intervalId;
      }
    }
  },
  beforeUnmount() {
    // Clear the interval when the component is destroyed
    clearInterval(this.$data.intervalId);
  }
};
</script>

<template>
  <div class="main">
    <div class="title">
      <div>INTERMISSION</div>
    </div>
    <div class="timer" @click="startTimer">
      <div>
        <span class="digits">{{ twoDigitFormat(minutes) }}</span>
        <span style="margin-top: -0.3em;">:</span>
        <span class="digits">{{ twoDigitFormat(seconds) }}</span>
      </div>
    </div>
  </div>
</template>


<style scoped>
.main
{
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  color: #5CA1AD;
}

.title
{
  font-size: 10em;
  padding: .5em .3em .3em;
}

.timer
{
  font-size: 20em;
  cursor: pointer;
}

.digits {
  font-weight: 800;
  font-family: "B612 Mono", monospace;
}

.title,
.timer
{
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
