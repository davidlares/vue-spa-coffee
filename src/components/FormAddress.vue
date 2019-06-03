<template>
  <div>
    <h1 class="title">Delivery details</h1>

    <h2 class="subtitle">
      Where should we send your freshly roasted coffee beans?
    </h2>

    <form @input="submit" class="form">
      <div class="form-group">
        <label class="form-label" for="delivery_name">Name</label>
        <input v-model="$v.form.recipient.$model" type="text" placeholder="Recipients Name" class="form-control" id="delivery_name">
        <div v-if="$v.form.recipient.$error" class="error">field is required</div>
      </div>

      <div class="form-group">
        <label class="form-label" for="address">Address</label>
        <textarea v-model="$v.form.address.$model" placeholder="London Street 470978 New England" rows="3" class="form-control" id="address"></textarea>
        <div v-if="$v.form.address.$error" class="error">field is required</div>
      </div>
    </form>
  </div>
</template>

<script>
  import {required} from 'vuelidate/lib/validators'
  export default {
    data () {
      return {
        form: {
          address: null,
          recipient: this.wizardData.name
        }
      }
    },
    validations: {
      form: {
        address: {
          required
        },
        recipient: {
          required
        }
      }
    },
    // with a previous keep alive - watch sets a listener every time a value changes
    // watch: {
    //   'wizardData.name' (value) {
    //     this.form.recipient = value
    //   }
    // },
    // Vue hook
    activated(){
      this.form.recipient = this.wizardData.name
    },
    methods: {
      submit() {
        // emmiting submited info
        this.$emit('update', {
          data: {
            recipient: this.form.recipient,
            address: this.form.address,
          },
          valid: !this.$v.$invalid // sending the validation through the emit object (works for wizard back button)
        })
      }
    },
    props: {
      wizardData: {
        type: Object,
        required: true
      }
    }
  }
</script>

<style scoped>

</style>
