<script>
import { ref } from 'vue'
import { useDate } from 'np-date-picker-vue-3'

export default {
	props: {
		format: { type: String, default: "yyyy-mm-dd" },
		calenderType: { type: String, default: "Nepali" },
		yearSelect: { type: Boolean, default: true },
		monthSelect: { type: Boolean, default: true },
		classValue: { type: String, default: "" },
		placeholder: { type: String, default: "" },
		modelValue: { type: String, default: "" },
	},
	setup(props) {
		const { days, date, visible, show } = useDate(props)
		const formData = ref('');

		function selectDay(dateData) {
			date.value = dateData;
			formData.value = date.value.format(props.format)
		}
		return {
			days, date, visible, show,
			formData,
			selectDay
		}
	}
}

</script>

<template>
	<div class="row justify-center">
		<div class="col-4">
			<q-input outlined class="q-my-md" v-model="formData" @focus="show" />
			<q-card v-if="visible" class="q-mb-md">
				<q-card-section>
					<q-btn
						v-for="(date, i) in days"
						:key="i"
						:label="date.day"
						@click="selectDay(date)"
						flat
						round
					/>
				</q-card-section>
			</q-card>
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

.day-box {
	outline: 1px solid red;
	padding: 12px;
	width: 40px;
	background: #eee;
}

.cursor-pointer {
	cursor: pointer;
}
</style>
