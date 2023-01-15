<script setup lang="ts">
import { Field, ErrorMessage } from 'vee-validate';
import * as yup from 'yup';
import FormWizard from './components/FormWizard.vue';
import FormStep from './components/FormStep.vue';

//import { inject } from 'vue';
import { ref } from 'vue';

//const currentIdx = inject('STEP_COUNTER').value++;
//const formStepIdx = inject('CURRENT_STEP_INDEX');
const formStepIdx = ref(0);
const validationSchema = [
  yup.object({
    fullName: yup.string().required().label('Full Name'),
    email: yup.string().required().email().label('Email Address'),
  }),
  yup.object({
    password: yup.string().min(8).required(),
    confirmPass: yup
        .string()
        .required()
        .oneOf([yup.ref('password')], 'Passwords must match'),
  }),
  yup.object({
    favoriteDrink: yup
        .string()
        .required()
        .oneOf(['coffee', 'tea', 'soda'], 'Choose a drink'),
  }),
];

function onSubmit(formData) {
  console.log('onSubmit - ', JSON.stringify(formData, null, 2));
}
function isActive( num ) {
  return formStepIdx.value+1 == num;
}
function onStepChanged(currentStepNum) {
  formStepIdx.value = currentStepNum;
}
</script>

<template>
  <div class="container py-5">
    <div class="row">
      <div class="col-md-12">
        <h2 class="text-center mb-5">Bootstrap 4 Multi-step Form Wizard</h2>
<!-- TAB -->
        <div class="wizard">
          <div class="wizard-inner">
            <div class="connecting-line"></div>
            <ul class="nav nav-tabs nav-fill mb-5" role="tablist">

              <li class="nav-item" role="presentation"> <!--  @click="change_step(1)" :class="{ active: is_active(1) }" -->
                <a href="#step1" data-toggle="tab" aria-controls="step1" role="tab" title="Step 1">
                  <span class="round-tab">
                      <i class="fa fa-pencil" v-show="isActive(1)"></i>
                      <i class="fa fa-check" v-show="!isActive(1)"></i>
                  </span>
                </a>
              </li>

              <li class="nav-item disabled" role="presentation"> <!-- @click="change_step(2)" :class="{ active: is_active(2) }" -->
                <a href="#step2" data-toggle="tab" aria-controls="step2" role="tab" title="Step 2">
                  <span class="round-tab">
                      <i class="fa fa-pencil" v-show="isActive(2)"></i>
                      <i class="fa fa-check" v-show="!isActive(2)"></i>
                  </span>
                </a>
              </li>
              <li class="nav-item disabled" role="presentation"> <!-- @click="change_step(3)" :class="{ active: is_active(3) }" -->
                <a href="#step3" data-toggle="tab" aria-controls="step3" role="tab" title="Step 3">
                  <span class="round-tab">
                    <i class="fa fa-pencil" v-show="isActive(3)"></i>
                    <i class="fa fa-check" v-show="!isActive(3)"></i>
                  </span>
                </a>
              </li>


            </ul>
          </div>
        </div>
<!-- /TAB -->

      </div>
    </div>

    <div class="row">
      <div class="col-md-12">
        <FormWizard :validation-schema="validationSchema" @submit="onSubmit" @stepChanged="onStepChanged">
          <FormStep>
            <Field name="fullName" type="text" class="form-control" placeholder="Type your Full name" />
            <ErrorMessage name="fullName" v-slot="{ message }">
              <div class="invalid-feedback d-block">{{ message }}</div>
            </ErrorMessage>

            <Field name="email" type="email" class="form-control" placeholder="Type your email" />
            <ErrorMessage name="email" v-slot="{ message }">
              <div class="invalid-feedback d-block">{{ message }}</div>
            </ErrorMessage>
          </FormStep>

          <FormStep>
            <Field name="password" type="password" class="form-control" placeholder="Type a strong one" />
            <ErrorMessage name="password" v-slot="{ message }">
              <div class="invalid-feedback d-block">{{ message }}</div>
            </ErrorMessage>

            <Field name="confirmPass" type="password" class="form-control" placeholder="Confirm your password"/>
            <ErrorMessage name="confirmPass" v-slot="{ message }">
              <div class="invalid-feedback d-block">{{ message }}</div>
            </ErrorMessage>
          </FormStep>

          <FormStep>
            <label>Select a drink</label>
            <Field name="favoriteDrink" as="select" class="form-control">
              <option value="tea">Tea</option>
              <option value="coffee">Coffee</option>
              <option value="soda">Soda</option>
            </Field>
            <ErrorMessage name="favoriteDrink" v-slot="{ message }">
              <div class="invalid-feedback d-block">{{ message }}</div>
            </ErrorMessage>
          </FormStep>
        </FormWizard>
      </div>
    </div>
  </div>

</template>

<style>
input,
select {
  margin: 10px 0;
  display: block;
}
</style>
