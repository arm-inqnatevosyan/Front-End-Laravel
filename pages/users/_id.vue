<template>
  <div>
    <div>
      <NavBars />
      <div>
        <h1 class="text-center text-2xl">
          User
        </h1>
      </div>
      <div v-for="user in users" :key="user.id" class="w-80 h-52 bg-sky-700 shadow-lg shadow-cyan-500/50  mx-auto mt-2">
        <div class="flex flex-col h-full items-center justify-center text-white font-bold">
          <h1>{{ user.email }}</h1>
          <h1>{{ user.name }}</h1>
          <button class="text-black font-bold" @click="openUserProfile">
            Back Users Page
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavBars from '@/components/NavBars.vue'
export default {
  components: { NavBars },
  validate ({ params }) {
    return /^\d+$/.test(params.id)
  },
  data () {
    return {
      users: []
    }
  },
  async mounted () {
    await this.$axios.$get('/api/postman/csrf')
    const response = await this.$axios.get(`/api/contacts/${this.$route.params.id}`)
    const user = response.data
    this.users = user
    return user
  },
  methods: {
    async openUserProfile () {
      await this.$axios.$get('/api/postman/csrf')
      await this.$router.push(`/api/contacts/${this.$route.params.id}`)
    }
  }
}
</script>
