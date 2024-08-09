<template>
    <div class="input-form">
        <div class="form-group timezone">
            <label for="start-date" class="form-label">Timezone</label>
            <select class="form-select" aria-label="Select timezone" v-model="targetTimeZone">
                <option v-for="timezone in timezones" :value="timezone"> {{ timezone }}</option>
            </select>
        </div>
        <div class="form-group start-date">
            <label for="start-date" class="form-label">Start Date</label>
            <VueDatePicker format="yyyy/MM/dd" type="date" id="start-date" v-model="startDate" />
        </div>
        <div class="form-group end-date">
            <label for="end-date" class="form-label">End Date</label>
            <VueDatePicker format="yyyy/MM/dd" type="date" id="end-date" v-model="endDate" />
        </div>
        <div class="form-group time-range">
            <label for="time-range-start" class="form-label">Time Range (Start Time)</label>
            <VueDatePicker time-picker id="time-picker" placeholder="xx:xx" v-model="timeRangeStart" />
        </div>
        <div class="form-group time-range">
            <label for="time-range-end" class="form-label">Time Range (End Time)</label>
            <VueDatePicker time-picker id="time-picker" placeholder="xx:xx" v-model="timeRangeEnd" />
        </div>
        <div class="generate-button-wrapper">
            <button class="btn btn-primary generate-button" @click="startProcess">Generate !</button>
        </div>
        <div class="generate-result">
            <textarea class="form-control" id="result" rows="10" v-model="resultText">
            </textarea>
        </div>
    </div>
</template>

<script>
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import moment from 'moment-timezone';
import random from 'random';

export default {
    name: 'GenerateForm',
    components: {
        VueDatePicker,
    },
    data() {
        return {
            startDate: '',
            endDate: '',
            timeRangeStart: '',
            timeRangeEnd: '',
            resultText: '',
            timezones: moment.tz.names(),
            targetTimeZone: moment.tz.guess(),
        }
    },
    methods: {
        startProcess() {
            const genResults = [];
            const startDate = moment.tz(this.startDate, this.targetTimeZone);
            const endDate = moment.tz(this.endDate, this.targetTimeZone);
            const dateDiff = endDate.diff(startDate, 'days');

            const startHour = moment.tz(this.timeRangeStart, this.targetTimeZone).hour();
            const endHour = moment.tz(this.timeRangeEnd, this.targetTimeZone).hour();

            const startMinute = moment.tz(this.timeRangeStart, this.targetTimeZone).minute();
            const endMinute = moment.tz(this.timeRangeEnd, this.targetTimeZone).minute();

            //Setup the minute before doing anything
            startDate.minute(startMinute);
            endDate.minute(endMinute);

            const targetDateTime = {
                startDate,
                endDate,
                startHour,
                endHour,
            };

            //Add the start date in date list.
            const dateList = [];
            dateList.push(startDate);

            //Get the all of the date (exclude first and last date) in range first.
            for (let i = 1; i < dateDiff; i++) {
                const newDate = startDate.clone();
                newDate.add(i, 'd');
                dateList.push(newDate);
            }
            dateList.push(endDate);
            for (let i = 0; i < dateList.length; i++) {
                const date = dateList[i].clone();
                const randomTime = this.generateRandomTime(date, targetDateTime);
                const newDateTime = {
                    date: randomTime.format('YYYY-MM-DD HH:mm:ss'),
                    timestamp: randomTime.unix(),
                };
                genResults.push(newDateTime);
            }
            genResults.sort(function (a, b) { return a - b });

            let resultText = '';
            for (let i = 0; i < genResults.length; i++) {
                const result = genResults[i];
                resultText = `${resultText} \n ${result['timestamp']} (${result['date']})`;
            }
            this.resultText = resultText;
        },
        generateRandomTime(date, targetDateTime) {
            const newDate = date.clone();
            let newHour = this.randomHour(targetDateTime.startHour, targetDateTime.endHour);
            let newMinute = this.randomMinute();
            newDate.hour(newHour);
            newDate.minute(newMinute);

            //If the timestamp over the time range, regen it
            while(newDate.unix() < targetDateTime.startDate.unix() || newDate.unix() > targetDateTime.endDate){
                newHour = this.randomHour(targetDateTime.startHour, targetDateTime.endHour);
                newMinute = this.randomMinute();
                newDate.hour(newHour);
                newDate.minute(newMinute);
            }
            //Return updated date
            return newDate;
        },
        randomHour(startHour, endHour) {
            const hour = random.int(startHour, endHour);
            return hour;
        },
        randomMinute() {
            const minute = random.int(0, 59);
            return minute;
        },
    },
}
</script>

<style lang="scss">
.form-group {
    margin-bottom: 10px;
}

.number {
    margin-bottom: 30px;
}

.generate-button-wrapper {
    text-align: center;
}

.generate-result {
    margin-top: 30px;
    margin-bottom: 30px;
}

/* Timepicker */
#start-date .dp--tp-wrap,
#end-date .dp--tp-wrap {
    display: none;
}

.time-range {
    .dp__pointer {
        padding-left: 20px;
    }

    .dp__input_icon {
        display: none;
    }
}
</style>