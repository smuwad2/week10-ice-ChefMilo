<script setup>
// Import BlogPost component
import blogPost from './subcomponents/BlogPost2.vue'
import axios from 'axios'
</script>

<script>
export default {
  data() {
    return {
      posts: [] 
    }  
  },
  computed: {
    baseUrl() {
      if (window.location.hostname == 'localhost') {
        return 'http://localhost:3000'
      } else {
        const codespace_host = window.location.hostname.replace('5173', '3000')
        return `https://${codespace_host}`
      }
    }
  },
  created() { 
    axios.get(`${this.baseUrl}/posts`)
      .then(response => {
        this.posts = response.data
        console.log(response.data)
      })
      .catch(error => {
        this.posts = [{ id: -1, subject: 'Error', entry: 'There was an error: ' + error.message, mood: 'neutral' }]
      })
  },
  methods: {
    async deletePost(id) {
      try {
        await axios.get(`${this.baseUrl}/deletePost`, { params: { id } })
        this.posts = this.posts.filter(p => p.id !== id)
      } catch (err) {
        alert('Failed to delete post: ' + err.message)
      }
    }
  }
}
</script>

<template>
  <div class="container my-4">
    <div class="row g-3">
      <div class="col-12 col-md-6 col-lg-4" v-for="p in posts" :key="p.id">
        <blogPost :subject="p.subject" :entry="p.entry" :mood="p.mood">
          <template #actions>
            <button class="btn btn-outline-danger btn-sm"
                    @click="deletePost(p.id)">
              Delete
            </button>
          </template>
        </blogPost>
      </div>
    </div>
  </div>
</template>