<template>
  <form @submit="onSubmit">
    <slot />

    <div class="row my-2">
<!--      <div class="col-md-6 offset-md-3">-->
      <div class="col text-center">
        <button v-if="hasPrevious" type="button" @click="goToPrev" class="btn btn-secondary">
          Previous
        </button>
        <button type="submit" class="btn btn-primary mx-2">{{ isLastStep ? 'Submit' : 'Next' }}</button>
      </div>
    </div>
    <div class="row my-2">
      <div class="col text-center">
        <pre>{{ values }}</pre>
      </div>
    </div>
  </form>
</template>

<script setup lang="ts">
import { useForm } from 'vee-validate';
import { ref, computed, provide } from 'vue';

const props = defineProps({
  validationSchema: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['submit', 'stepChanged']); //, 'stepChanged'
const currentStepIdx = ref(0);
const stepCounter = ref(0);
provide('CURRENT_STEP_INDEX', currentStepIdx);
provide('STEP_COUNTER', stepCounter);

const isLastStep = computed(() => {
  return currentStepIdx.value === stepCounter.value - 1;
});

const hasPrevious = computed(() => {
  return currentStepIdx.value > 0;
});

const currentSchema = computed(() => {
  return props.validationSchema[currentStepIdx.value];
});

const { values, handleSubmit } = useForm({
  validationSchema: currentSchema,
  // turn this on so each step values won't get removed when you move back or to the next step
  keepValuesOnUnmount: true,
});

const onSubmit = handleSubmit((values) => {
  if (!isLastStep.value) {
    currentStepIdx.value++;
    emit('stepChanged', currentStepIdx.value);
    return;
  }
  emit('submit', values);
});

function goToPrev() {
  if (currentStepIdx.value === 0) {
    return;
  }
  currentStepIdx.value--;
  emit('stepChanged', currentStepIdx.value);
}
</script>
