<template>
  <Header />
  <main>
    <div class="main-content">
      <div class="container">
        <div class="title">
          Timefaker
        </div>
        <div class="description">
          A tool for generate random timestamp with specified time range.
        </div>
        <div class="description-2">
          (Example: Generate the timestamp between 2024/08/06 - 2024/08/07, every 10:00 - 18:00.)
        </div>
        <div class="input-form">
          <div class="form-group start-date">
            <label for="start-date" class="form-label">Start Date</label>
            <VueDatePicker format="yyyy/MM/dd" type="date" id="start-date"v-model="form.startdate" />
          </div>
          <div class="form-group end-date">
            <label for="end-date" class="form-label">End Date</label>
            <VueDatePicker format="yyyy/MM/dd" type="date" id="end-date" v-model="form.endDate" />
          </div>
          <div class="form-group time-range">
            <label for="time-range-start" class="form-label">Time Range (Start Time)</label>
            <VueDatePicker time-picker id="time-picker" v-model="form.timeRangeStart" />
          </div>
          <div class="form-group time-range">
            <label for="time-range-end" class="form-label">Time Range (End Time)</label>
            <VueDatePicker time-picker id="time-picker" v-model="form.timeRangeEnd" />
          </div>
          <div class="form-group number">
            <label for="number" class="form-label">How many result would you want?</label>
            <input id="number" type="number" class="form-control" v-model="form.number"/>
          </div>
          <div class="generate-button-wrapper">
            <button class="btn btn-primary generate-button">Generate !</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import Header from '@/components/Header.vue';

export default {
  name: 'App',
  components: {
    Header,
    VueDatePicker,
  },
  data(){
    return {
      form: {
        startDate: '',
        endDate: '',
        timeRangeStart: '',
        timeRangeEnd: '',
        number: 1,
      },
    }
  },
}
</script>

<style lang="scss">
.title{
  font-size: 3em;
  font-weight: bold;
  text-align: center;
  margin-top: 50px;
  margin-bottom: 10px;
}
.description{
  font-size: 2em;
  text-align: center;
  margin-bottom: 5px;
}
.description-2{
  margin-bottom: 30px;
  text-align: center;
}
.form-group{
  margin-bottom: 10px;
}
.number{
  margin-bottom: 30px;
}
.generate-button-wrapper{
  text-align: center;
}

/* Timepicker */
#start-date .dp--tp-wrap, #end-date .dp--tp-wrap{
  display: none;
}
.time-range{
  .dp__input_icon{
    display: none;
  }
}
</style>
