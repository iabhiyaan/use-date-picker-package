<script>
import { ref } from "vue";
import {NepaliDatePicker} from 'np-date-picker-vue-3';

import CDate from './components/CDate.vue';

export default {
  components: {
    NepaliDatePicker,
    CDate
  },
  props: {

  },
  setup() {
    const englishDate = ref('')
    const nepaliDate = ref('')
    const englishDate1 = ref('')
    const nepaliDate1 = ref('')
    return {
      englishDate,
      nepaliDate,
      englishDate1,
      nepaliDate1,
    }
  }

}

</script>

<template>
  <div id="app">
    <CDate calenderType="Nepali" />
    <div style="display: flex">
      <div id="english-date">
        <label>English Nepali DatePicker :</label>
        <NepaliDatePicker v-model="englishDate" />
        <div>{{ englishDate }}</div>
      </div>
      <div style="margin-left: 30px" id="nepali-date">
        <label>Nepali DatePicker :</label>
        <NepaliDatePicker
          calenderType="Nepali"
          format="yyyy-mm-dd"
          classValue="form-control"
          v-model="nepaliDate"
        />
        <div>{{ nepaliDate }}</div>
      </div>
      <div style="margin-left: 30px" id="english-nepali-without-select">
        <label>English Nepali DatePicker (without select):</label>
        <NepaliDatePicker v-model="englishDate1" :monthSelect="false" :yearSelect="false" />
        <div class="mt-4">{{ englishDate1 }}</div>
      </div>
      <div style="margin-left: 30px" id="nepali-without-select">
        <label>Nepali DatePicker (without select):</label>
        <NepaliDatePicker
          calenderType="Nepali"
          format="yyyy-mm-dd"
          classValue="form-control"
          v-model="nepaliDate1"
          :monthSelect="false"
          :yearSelect="false"
        />
        <div class="mt-4">{{ nepaliDate1 }}</div>
      </div>
    </div>
  </div>
</template>