<template>
  <div class="container">
    <p v-if="comments.length === 0">
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <div v-else class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <p><strong>{{comment.email}}</strong></p>
            <small>{{comment.text}}</small>
          </div>
        </li>
      </ul>
    </div>
    <app-loader v-if="loading"></app-loader>
  </div>
</template>

<script>
import AppLoader from "@/components/AppLoader";
export default {
  name: "AppComments",
  data() {
    return {
      comments: [],
      loading: false,
    }
  },
  methods: {
    async loadComments() {
      this.loading = true
      const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42', {
        type: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      })
      this.comments = await response.json()
      this.loading = false
    }
  },
  components: {AppLoader}
}
</script>

<style scoped>

</style>