<template>
  <div>
    <div class="login"><LoginForm /></div>
    <button @click="redirectToVotepage">Vote</button>
  </div>
</template>

<script>
import LoginForm from '@/components/LoginForm.vue'

export default {
  name: 'LoginFormView',
  components: {
    LoginForm
  },
  methods: {
    redirectToVotepage () {
      this.$router.push({ name: 'Votepage' })
    }
  }
}
</script>

<style scoped>
  .login {
    display: flex;
    justify-content: center;
    margin-top: 2rem;
  }
  button {
    display: block;
    margin: auto;
  }
</style>
