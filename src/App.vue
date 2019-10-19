<template>
  <div id="app">
    <div class="ui active dimmer" v-if="loading">
      <div class="ui laoder" ></div>
    </div>
    <div class="ui four column grid">
      <div class="column" v-for="user in users" :key="user.id">
        <div class="ui card">
          <div class="content">
            <input type="text" v-model="user.name">
            <input type="button" @click="save(user)" value="update">
            <input type="button" @click="destroy(user)" value="delete">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      loading: false
    }
  },
  methods: {
    save(user) {
      this.loading = true
      this.$userApi.update({
        id: user.id
      },{
        name: user.name
      }).then((response) => {
        console.log('success', response)
      }, (response) => {
        console.log('error', response)
      }).then(_ => {
        console.log(_)
        this.loading = false
      })
    },
    destroy(user) {
      this.loading = true
      this.$userApi.remove({
        id: user.id
      }).then((response) => {
        console.log('success', response)
        this.users = this.users.filter(u => u !== user)
      }, (response) => {
        console.log('error', response)
      }).then(_ => {
        console.log(_)
        this.loading = false
      })
    }
  },
  mounted() {
    this.$userApi = this.$resource('users{/id}')
    this.$userApi.query().then((response) => {
      this.users = response.data
    }, (response) => {
      console.log('error', response) 
    })
  }
}
</script>
