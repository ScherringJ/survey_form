<template>
  <div class="mt-3 mx-sm-auto text-center card w-50 shadow-sm bg-body-tertiary rounded">
    <div class="card-body">
        
        <Transition name="slide-fade" @after-leave="show = true">
          <div class="text-center" v-if="show">
            <p>
              <strong>{{ questions[cptQuestion].question }}</strong>
            </p>
          </div>
        </Transition>

        <div class="text-center mb-3">
          
          <div class="d-inline mx-3">
            Bad
          </div>

          <div class="form-check form-check-inline" v-for="index in 5" :key="index">
            <input class="form-check-input" type="radio" :id=index  v-model="questions[cptQuestion].val" :value=index />
            <label class="form-check-label" :for=index>{{ index }}</label>
          </div>

          <div class="d-inline me-4">
            Excellent
          </div>

        </div>

        <div class="d-grid gap-2 d-md-flex justify-content-md-center">
          <button class="btn btn-primary" type="button" @click="previousQuestion" v-if="!notPrevious"> Previous </button>
          <button class="btn btn-primary" type="button" @click="nextQuestion" v-if="!isFinish"> Next </button>
          <button class="btn btn-success" type="button" @click="submit" v-else> Submit </button>
        </div>
  
    </div>

  </div>
</template>

<script setup>
import { ref, computed} from 'vue';
import survey from './data/survey.json';
import axios from 'axios';

const questions = ref(survey)

const cptQuestion = ref(0)
const show = ref(true)


const isFinish = computed(() => {
  return cptQuestion.value == questions.value.length - 1
})

const notPrevious = computed(() => {
  return cptQuestion.value == 0
})


/**
 * Go to the next question
 */
const nextQuestion = () => {
  if (!isFinish.value) {
    show.value=false
    cptQuestion.value++
  }
}


/**
 * Go to the previous question
 */
const previousQuestion = () => {
  if (!notPrevious.value) {
    show.value=false
    cptQuestion.value--
  }
}


/**
 * Submit survey to the api 
 */
const submit = () => {
  axios({
    method: 'post',
    url: '',
    data: questions.value
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
}


</script>

<style>


.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(-20px);
  opacity: 0;
}

.slide-fade-enter-from {
  transform: translateX(20px);
  opacity: 0;
}


</style>
