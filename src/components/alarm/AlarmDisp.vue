<script>
import SaveAlarm from '@/components/alarm/SaveAlarm.vue'

export default {
    name: 'AlarmDisp',

    components: {
        SaveAlarm
    },

    props: ['alarm'],

    data() {
        return {
            forms: false
        }
    },

    computed: {
        hour() {
            return ('0' + this.alarm.hour).slice(-2);
        },
        
        minute() {
            return ('0' + this.alarm.minute).slice(-2);
        }
    },

    methods: {
        editAlarmForm() {
            this.forms = !this.forms;
        },

        editAlarm() {
            const alarm = this.alarm;
            const post_data = {
                hour: document.querySelector("[name='hour']").value,
                minute: document.querySelector("[name='minute']").value
            };
            this.$emit('editAlarm', {
                alarm,
                post_data
            })
        },
        
        removeAlarm() {
            const alarm = this.alarm;
            this.$emit('removeAlarm', {
                alarm
            })
        },
    }
}
</script>

<template>
    <div>
        <div class="alarm-disp-bloc" v-if="!forms">
            <h3 class="alarms"> {{ hour }} : {{  minute }} </h3>
            <i class='bx bxs-edit-alt bx-tada-hover' @click="editAlarmForm"></i>
            <i class='bx bxs-trash bx-tada-hover' @click="removeAlarm"></i>
        </div>
        <SaveAlarm v-else :submitData="editAlarm" :cancel="editAlarmForm" :currentAlarmHour="alarm.hour" :currentAlarmMinute="alarm.minute"/>
    </div>
</template>

<style>
.alarm-disp-bloc {
    padding: 10px;
    margin-bottom: 20px;
    margin-top: 20px;
    width: 120px;
    height: 65px;
    background-color: white;
    border-radius: 20px 5px;
    border: .05em solid rgba(0, 0, 0, .7);
    box-shadow: -2px 3px 5px;
}

.alarms {
    font-size: 1.75em;
    margin: 0;
    margin-bottom: 5px;
    margin-top: 5px;
}

i.bxs-edit-alt, i.bxs-trash {
    margin-bottom: 10px;
    margin-left: 5px;
    margin-right: 5px;
    font-size: 1.2em;
}
</style>