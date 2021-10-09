<script>
export default {
    name: 'SaveAlarm',

    props: ["submitData", "cancel", "currentAlarmHour", "currentAlarmMinute"],
}
</script>

<template>
    <div>
        <form id='save-alarm-form'>
            <select name="hour" :value="currentAlarmHour">
                <option v-for="hour in Array(24).keys()" :key="hour">{{ hour }}</option>
            </select>
            <select name="minute" :value="currentAlarmMinute">
                <option v-for="minute in Array(60).keys()" :key="minute">{{ minute }}</option>
            </select>
            <i class='bx bxs-save' @click="submitData"></i>
            <i class='bx bx-x' @click="cancel"></i>
        </form>
    </div>
</template>

<style>
</style>