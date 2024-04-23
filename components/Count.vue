<template>
  <div class="countdown">
    <section class="time timeShow d-flex">
      <main v-if="timeLeft" class="d-flex time-container">
        <div class="time-box">
          <span>{{ days }} </span> <span>Days</span>
        </div>
        <div class="time-box">
          <span>{{ hours }} </span> <span>Hours</span>
        </div>

        <div class="time-box">
          <span>{{ minutes }} </span> <span>Min</span>
        </div>
        <div class="time-box">
          <span>{{ seconds }} </span> <span>Secs</span>
        </div>


        <button @click="stopCountdown">Stop Countdown</button>
      </main>
      <div v-else>
        <span>Countdown has ended or is stopped!</span>
      </div>
    </section>
    <section class="time timeSet d-flex">
      <div>
        <input type="number" v-model.number="inputMinutes" placeholder="Enter minutes" />
        <button @click="startCountdown">Start Countdown</button>
      </div>
    </section>

  </div>
</template>


<script>
export default {
  name: "Countdown",
  data() {
    return {
      targetDate: null,
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      timeLeft: false,
      timer: null,
      inputMinutes: 0, // User input for minutes
    };
  },
  methods: {
    updateCountdown() {
      const now = new Date().getTime();
      const distance = this.targetDate - now;

      this.days = Math.floor(distance / (1000 * 60 * 60 * 24));
      this.hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      this.minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      this.seconds = Math.floor((distance % (1000 * 60)) / 1000);

      if (distance < 0) {
        this.clearTimer();
        this.timeLeft = false;
      }
    },
    startCountdown() {
      if (this.timer) {
        this.clearTimer();
      }
      if (this.inputMinutes > 0) {
        const now = new Date().getTime();
        this.targetDate = now + this.inputMinutes * 60000; // Convert minutes to milliseconds
        this.timer = setInterval(this.updateCountdown, 1000);
        this.timeLeft = true;
      } else {
        alert("Please enter a positive number of minutes.");
      }
    },
    stopCountdown() {
      this.clearTimer();
      this.timeLeft = false;
    },
    clearTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    }
  }
};
</script>

<style scoped>
*{
  background-color:  #1995AD;
  color: #F1F1F2;

}
.d-flex {
  display: flex;
  justify-content: center;
  align-items: center;
}


.time-box {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 10px;
}
.time-container{
  gap: 10px;
}

.countdown {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

}

.timeShow {
  margin-bottom: 20px;
}

.time {
  border: 1px solid#A1D6E2;
  width: 600px;
  height: 150px;
  box-shadow:  5px solid#A1D6E2;
}

.countdown span {
  font-size: 35px;
  margin-right: 10px;
}

.countdown button {
  padding: 8px 16px;
  margin-top: 10px;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

input[type="number"] {
  margin-right: 10px;
  padding: 8px;
  font-size: 16px;
  width: 200px;
}
</style>
