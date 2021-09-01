<template>
  <b-container class="form-signin">
    <b-row
      align-v="center"
      class="justify-content-center"
      align-h="center"
      style="height: 100vh"
    >
      <b-col md="4">
        <h2 class="text-center">Login</h2>
        <b-form @submit.stop.prevent="onSubmit">
          <b-form-group id="input-group-1" label="E-mail:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="email"
              type="email"
              placeholder="E-Mail"
              :class="{ 'is-invalid': submitted && $v.email.$error }"
            ></b-form-input>
            <div v-if="submitted && $v.email.$error" class="invalid-feedback">
              <span v-if="!$v.email.required">Email es requerido.</span>
              <span v-if="!$v.email.email">
                Por favor ingrese un Email válido.
              </span>
            </div>
          </b-form-group>
          <b-form-group
            id="input-group-3"
            label="Contraseña:"
            label-for="input-3"
          >
            <b-form-input
              id="input-3"
              v-model="password"
              type="password"
              :class="{ 'is-invalid': submitted && $v.password.$error }"
            ></b-form-input>
            <div
              v-if="submitted && !$v.password.required"
              class="invalid-feedback"
            >
              Contraseña es requerida.
            </div>
          </b-form-group>
          <b-row class="justify-content-between my-auto m-0">
            <b-button type="submit" variant="success">Ingresar</b-button>
            <b-link to="registro" class="btn btn-primary">Registrarme</b-link>
          </b-row>
        </b-form>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import { required, email } from 'vuelidate/lib/validators'
export default {
  data() {
    return {
      submitted: false,
      email: null,
      password: null,
    }
  },
  validations: {
    email: {
      required,
      email,
    },
    password: {
      required,
    },
  },
  methods: {
    async onSubmit() {
      try {
        this.submitted = true
        // stop here if form is invalid
        this.$v.$touch()
        if (this.$v.$invalid) {
          return
        } else {
          await this.$auth.loginWith('jwt', {
            data: {
              email: this.email,
              password: this.password
            }
          })
        }
      } catch (err) {
        console.log(err)
        this.$bvToast.toast('Usuario no existe', {
          title: `Error de acceso`,
          variant: 'danger',
          solid: true
        })
      }
    },
  },
}
</script>
