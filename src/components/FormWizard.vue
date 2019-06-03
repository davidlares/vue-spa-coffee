<template>
  <div>
    <!-- dnyamic component wizard -->
    <keep-alive>
      <component :is="currentStep" @update="processStep" :wizard-data="form" ref="currentStep"></component>
    </keep-alive>
    <!-- progress bar -->
    <div class="progress-bar">
      <div :style="`width: ${progress}%;`"></div>
    </div>
    <!-- Actions -->
    <div class="buttons">
      <button @click="goBack" v-if="currentStepNumber > 1" class="btn-outlined">Back</button>
      <button @click="goNext" class="btn" :disabled ="!canGoNext">Next</button>
    </div>
    <!-- debugging object -->
    <pre><code>{{form}}</code></pre>
  </div>
</template>

<script>
  import FormPlanPicker from './FormPlanPicker'
  import FormUserDetails from './FormUserDetails'
  import FormAddress from './FormAddress'
  import FormReviewOrder from './FormReviewOrder'
  export default {
    name: 'FormWizard',
    components: {
      FormPlanPicker,
      FormUserDetails,
      FormAddress,
      FormReviewOrder
    },
    data () {
      return {
        currentStepNumber: 1,
        canGoNext: false,
        // dynamic forms steps
        steps: ['FormPlanPicker', 'FormUserDetails','FormAddress', 'FormReviewOrder'],
        form: {
          plan: null,
          email: null,
          name: null,
          password: null,
          address: null,
          recipient: null,
          chocolate: false,
          otherTreat: false
        }
      }
    },
    computed: {
      // setting the steps as computed props
      length() {
        return this.steps.length
      },
      // the actual step will be a computed prop
      currentStep() {
        return this.steps[this.currentStepNumber - 1]
      },
      progress () {
        return this.currentStepNumber/this.length * 100
      }
    },
    methods: {
      // receiving emmit process
      processStep(step){
        // Object assign copies the whole properties of a object to another
        Object.assign(this.form, step.data) // payload
        // wizard Next validator
        this.canGoNext = step.valid
      },
      goBack () {
        this.currentStepNumber--
        this.canGoNext = true
      },
      goNext () {
        this.currentStepNumber++
        // wizard Next validator
        // this.canGoNext = false
        // using refs to persist updated data more than one step back

        // executes after the next DOM update cycle
        this.$nextTick(() => {
          // using prev solution with Vuelidate
          this.canGoNext = !this.$refs.currentStep.$v.$invalid
          // this.$refs.currentStep.submit() // called after each NEXT event
        })
      }
    }
}
</script>
