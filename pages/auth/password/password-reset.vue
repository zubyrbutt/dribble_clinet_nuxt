<template>
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
        Reset Password
      </h1>
      <form class="auth-form" @submit.prevent="submit">
        <alert-success :form="form">
          {{status}}
        </alert-success>
        <div class="form-group">
          <input
            readonly
            v-model.trim="form.email"
            name="email"
            type="text"
            class="form-control"
            :class="{'is-invalid': form.errors.has('email')}"
            placeholder="Email"
          >
          <has-error :form="form" field="email"></has-error>
        </div>

        <div class="form-group">
          <input
            v-model.trim="form.password"
            name="password"
            type="text"
            class="form-control"
            :class="{'is-invalid': form.errors.has('password')}"
            placeholder="New password"
          >
          <has-error :form="form" field="password"></has-error>
        </div>

        <div class="form-group">
          <input
            v-model.trim="form.password_confirmation"
            name="password_confirmation"
            type="text"
            class="form-control"
            :class="{'is-invalid': form.errors.has('password_confirmation')}"
            placeholder="Conform password"
          >
        </div>

        <div class="text-right">
          <button :disabled="form.busy" type="submit" class="btn btn-primary">
            <span v-if="form.busy">
              <i class="fas fa-spinner fa-spin"></i>
            </span>
            Reset password
          </button>
        </div>

      </form>
    </div>
  </section>
</template>

<script>
  export default {

    data() {
      return {
        status: '',
        form: this.$vform({
          email: '',
          password: '',
          password_confirmation: '',
          token: ''
        })
      }
    },
    methods: {
      submit() {
        this.form.post(
          `/password/reset`
        ).then(res => {
          this.status = res.data.status
          this.form.reset()
        })
      }
    },
    created() {
      this.form.email = this.$route.query.email
      this.form.token = this.$route.params.token
    }
  }
</script>
