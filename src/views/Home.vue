<template>
  <div class="home">
    <div class="disp-current-time">
      <p id="current-date">{{ currentDate }}</p>
      <p id="current-time">{{ currentTime }}</p>
    </div>
    <div class="disp-alarms">
      <h3>Alarms</h3>
      <AlarmDisp v-for="alarm in alarms" :key="alarm._id" :alarm="alarm" @removeAlarm="removeAlarm" @editAlarm="editAlarm"/>
      <div class="alarm-disp-bloc" v-if="!forms">
        <i class='bx bx-plus bx-border-circle bx-tada-hover' @click="createAlarmForm"></i>
      </div>
      <SaveAlarm v-else :submitData="createAlarm" :cancel="createAlarmForm" :currentAlarmHour="0" :currentAlarmMinute="0"/>
    </div>
  </div>
</template>

<script>
import SaveAlarm from '@/components/alarm/SaveAlarm.vue'
import AlarmDisp from '@/components/alarm/AlarmDisp.vue'
import axios from "axios";
import { server } from "../helper";

const month = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre']

const week = ['Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi', 'Dimanche']

function completeByZero(number, length) {
    return ('000' + number).slice(-length)
}

export default {
  name: 'Home',
  
  components: {
    AlarmDisp,
    SaveAlarm
  },

  data() {
    return {
      alarms: [],
      currentTime: "",
      currentDate: "",
      forms: false
    };
  },

  created() {
    this.fetchAlarms()
    setInterval(this.updateTime, 1000)
  },

  methods: {
    fetchAlarms() {
      axios.get(`${server.baseURL}/alarm`)
      .then(response => {
        this.alarms = response.data
      })
    },

    updateTime() {
      let currentTime = new Date()
      
      this.currentTime = completeByZero(currentTime.getHours(), 2) + ':' 
      + completeByZero(currentTime.getMinutes(), 2) + ':'
      + completeByZero(currentTime.getSeconds(), 2);

      this.currentDate = week[currentTime.getDay()] + ' ' + currentTime.getDate() + ' ' + month[currentTime.getMonth()] + ' ' + completeByZero(currentTime.getFullYear(), 4)
    },

    createAlarm() {
      const post_data = {
        hour: document.querySelector("[name='hour']").value,
        minute: document.querySelector("[name='minute']").value
      };
      axios.post(`${server.baseURL}/alarm/`, post_data).then(response => {
        console.log(response.data);
        window.location.reload();
      })
    },

    createAlarmForm() {
      this.forms = !this.forms
    },

    editAlarm(payload) {
      axios.put(`${server.baseURL}/alarm/${payload.alarm._id}`, payload.post_data).then(response => {
        console.log(response.data);
        window.location.reload();
      })
    },

    removeAlarm(payload) {
      axios.delete(`${server.baseURL}/alarm/${payload.alarm._id}`).then(response => {
        console.log(response.data);
        window.location.reload();
      })
    },
  }
}
</script>

<style>
* {
  margin: 0;
}

body {
  background-color: rgba(0, 0, 0, .1);
}

.disp-alarms {
  margin: auto;
  margin-top: 50px;
  margin-bottom: 50px;
  padding: 10px;
  min-width: 30%;
  max-width: 60%;
  min-height: 500px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: flex-start;
  column-gap: 100px;
  border: solid rgba(0, 0, 0, .7);
  border-radius: 10px;
  background-color: rgba(0, 0, 0, .2);
}
.disp-alarms > h3 {
  width: 100%;
  text-decoration: underline;
}

i.bx-plus {
  font-size: 2.5em;
  background-color: white;
  /* box-shadow: -2px 3px 5px; */
  border: none;
}

.disp-current-time {
  width: 30%;
  margin: auto;
  padding: 10px;
  padding-left: 20px;
  padding-right: 20px;
  margin-top: 20px;
  margin-bottom: 20px;
  background-color: white;
  border-radius: 10px;
  box-shadow: -3.5px 5px 10px;
  word-wrap: break-word;
}

#current-date {
  font-size: 1em;
}
  
#current-time {
  margin: 10px;
  font-size: 5em;
}

@media only screen and (max-width: 768px) {
  .disp-current-time {
    width: 50%;
  }
  #current-time {
    font-size: 2.15em;
    font-weight: 600;
  }
}
</style>