<template>
  <div class="row" v-if="shouldShow">
    <div class="col-md-6 offset-md-3">
      <div class="card card-outline-secondary">
        <div class="card-header">
          <h3 class="mb-0">Step {{ formStepIdx+1 }}</h3>
        </div>
        <div class="card-body">
          <slot />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, inject } from 'vue';

const currentIdx = inject('STEP_COUNTER').value++;
const formStepIdx = inject('CURRENT_STEP_INDEX');

const shouldShow = computed(() => {
  return currentIdx === formStepIdx.value;
});
</script>
