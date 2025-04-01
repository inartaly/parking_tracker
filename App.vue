<script>
export default {
  data() {
    return {
      parkingFloor: "",
      timeIn: "",
    };
  },
  methods: {
    checkIn() {
      this.timeIn = new Date().toLocaleTimeString();
    },
    saveData() {
      localStorage.setItem(
        "parkingData",
        JSON.stringify({
          floor: this.parkingFloor,
          timeIn: this.timeIn,
        })
      );
    },
    reset() {
      this.parkingFloor = "";
      this.timeIn = "";
      localStorage.removeItem("parkingData");
    },
    scheduleNotification() {
      const now = new Date();
      const fivePM = new Date();
      fivePM.setHours(17, 0, 0);
      const delay = fivePM - now;

      if (delay >= 0) {
        setTimeout(() => {
          new Notification("Parking Info", {
            body: `You parked on floor ${this.parkingFloor}.`,
          });
        }, delay);
      }
    },
  },
  mounted() {
    // Trigger notifications after app loads
    if (Notification.permission !== "granted") {
      Notification.requestPermission();
    }
    this.scheduleNotification();
  },
};
</script>


<template>
  <div>
    <h1>Parking Floor Tracker</h1>
    <input type="text" v-model="parkingFloor" placeholder="Enter Parking Floor">
    <button @click="checkIn">Check-In</button>
    <p>Time In: {{ timeIn }}</p>
    <button @click="saveData">Save</button>
    <button @click="reset">Reset</button>
  </div>
</template>


<style scoped>
h1 {
  font-size: 1.5em;
  text-align: center;
}
button {
  margin: 5px;
  padding: 10px;
  border: none;
  background-color: #3498db;
  color: white;
  font-weight: bold;
}
</style>
