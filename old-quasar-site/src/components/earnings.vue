<template>
    <div class="row items-start q-gutter-lg earnings">
      <div class="col">
        <h5 class="q-my-none">Income</h5>
        <q-input v-model="income" label="Yearly Income" type="number" />
        <q-input v-model="startAge" label="Start Age" type=number />
        <q-input v-model="retirementAge" label="Retirement Age" type="number" />
        <q-input v-model="interest" label="Savings Interest %" type="number" />
      </div>
      <div class="col">
        <h5 class="q-mb-none">Deductions</h5>
        <q-input v-model="incomeTax" label="Income Tax %" type="number" />
        <q-input v-model="deductions" label="Yearly Deductions" type="number" />
        <q-btn color="primary" label="Save" @click="save" />
      </div>
    </div>
</template>

<script>
import { useStore } from 'vuex'
import { computed, ref } from 'vue'
import Income from 'src/models/income'

export default {
  name: 'earnings',
  setup() {
    const income = ref(60000)
    const deductions = ref(50000)
    const startAge = ref(25)
    const retirementAge = ref(65)
    const interest = ref(2)

    const store = useStore()

    const save = function() {
      store.dispatch('graph/setTask', new Income('Basic Task', income.value, deductions.value, startAge.value, retirementAge.value, interest.value))
    }

    const incomeTax = computed(() => {
      if (income.value < 18200) {
        return 0
      } else if (income.value > 18201 && income.value < 37000) {
        return 19
      } else if (income.value > 37001 && income.value < 90000) {
        return 32
      } else if (income.value > 90001 && income.value < 180000) {
        return 37
      } else {
        return 45
      }
    })

    return {
      save,
      income,
      deductions,
      startAge,
      retirementAge,
      interest,
      incomeTax
    }
  }
}
</script>

<style scoped>
  .earnings {
    align-items: baseline;
  }
</style>
