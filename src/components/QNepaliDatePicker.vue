<script>
import {computed} from 'vue'
import {useDate} from 'np-date-picker-vue-3'

// const { ENGLISH_WEEK } = CONSTANT
export default {
  props: {
    calenderType: {type: String, default: "Nepali"},
    format: {
      type: String,
      default(rawProps) {
        if (rawProps.calenderType === 'English') {
          return 'YYYY-MM-DD'
        }
        return 'yyyy-mm-dd'
      },
    },
    yearSelect: {type: Boolean, default: true},
    monthSelect: {type: Boolean, default: true},
    classValue: {type: String, default: ""},
    placeholder: {type: String, default: ""},
    modelValue: {type: String, default: ""},
  },
  setup(props, {emit}) {

    const formData = computed({
      get: () => props.modelValue,
      set: (val) => emit('update:modelValue', val)
    })

    const {
      getToday,
      selectDate,
      getMonthsList,
      numberOfYears,
      startingYear,
      formatEnglish,
      getNepaliYears,
    } = useDate(props);
    const monthsOpts = computed(() => getMonthsList.value.map((month, i) => {
      return {
        id: i,
        month
      }
    }))
    const filledYears = Array.from({length: numberOfYears.value}, (x, i) => ++i)

    const yearOpts = computed(() => {
      return filledYears.map((yearCount) => {
        return {
          id: startingYear.value + (yearCount - 1),
          year: formatEnglish.value ? startingYear.value + (yearCount - 1) : getNepaliYears(startingYear.value + (yearCount - 1))
        }
      })
    })

    function today() {
      formData.value = getToday()
    }

    function selectDay(dayDate) {
      formData.value = selectDate(dayDate)
    }

    return {
      formData,
      /* use date start */
      ...useDate(props),
      /* use date end */
      today,
      selectDay,
      monthsOpts,
      yearOpts
    }
  }
}

</script>

<template>
  <div class="row justify-center">
    <div class="col-4">
      <q-input class="q-mb-md" outlined v-model="formData">
        <template v-slot:append>
          <q-icon name="event" class="cursor-pointer">
            <q-popup-proxy cover>
              <div style="min-width: 290px; max-width: 300px;">
                <q-card-section class="bg-primary text-white">
                  <div class="row items-center no-wrap">
                    <div class="col">
                      <div class="text-subtitle2">{{ formattedYear }}</div>
                      <div class="text-h6">{{ formattedDate }}</div>
                    </div>

                    <div class="col-auto">
                      <q-btn round flat icon="calendar_month" @click="today"/>
                    </div>
                  </div>

                </q-card-section>
                <q-separator/>
                <div class="flex items-center justify-between">
                  <div class="flex-1">
                    <q-btn
                        flat
                        round
                        icon="chevron_left"
                        @click="prev"
                    />
                  </div>
                  <div v-if="formattedYearOrMonth" class="flex-1">
                    <span>{{ formattedYearOrMonth }} </span>
                  </div>
                  <q-select
                      v-if="monthSelect"
                      :options="monthsOpts"
                      v-model="monthValue"
                      @update:model-value="monthSelectChange"
                      emit-value
                      map-options
                      option-label="month"
                      option-value="id"
                      borderless
                  />
                  <q-select
                      v-if="yearSelect"
                      :options="yearOpts"
                      v-model="yearValue"
                      @update:model-value="yearSelectChange"
                      emit-value
                      map-options
                      option-label="year"
                      option-value="id"
                      borderless
                  />
                  <div class="flex-1">
                    <q-btn
                        flat
                        round
                        icon="navigate_next"
                        @click="next"
                    />
                  </div>
                </div>
                <q-card-section>
                  <div class="grid-calendar">
                    <div
                        v-for="(weekday, w) in weekdays"
                        :key="w"
                    >
                      {{ weekday }}
                    </div>
                  </div>

                  <div class="grid-calendar">
                    <q-btn
                        v-for="(date, i) in days"
                        :key="i"
                        :label="formatEnglish ? date.day : getNepaliDays(date)"
                        @click="selectDay(date)"
                        flat
                        round
                        class="day-btn"
                    />
                  </div>
                </q-card-section>
              </div>

            </q-popup-proxy>
          </q-icon>
        </template>
      </q-input>

    </div>
  </div>
</template>

<style>
.flex {
  display: flex;
}

.flex-wrap {
  flex-wrap: wrap;
}

.cursor-pointer {
  cursor: pointer;
}

.grid-calendar {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  justify-content: center;
  text-align: center;
}

.day-btn {
  min-width: 2em !important;
}

.flex-1 {
  flex: 1;
}

</style>
