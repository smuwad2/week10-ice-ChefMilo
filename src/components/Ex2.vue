<script>
import blogPost from './subcomponents/BlogPost.vue'
import axios from 'axios'

export default {
  components: { 'blog-post': blogPost },
  data() {
    return { posts: [] }
  },
  computed: {
    baseUrl() {
      if (window.location.hostname === 'localhost') return 'http://localhost:3000'
      const codespace_host = window.location.hostname.replace('5173', '3000')
      return `https://${codespace_host}`
    }
  },
  created() {
    axios.get(`${this.baseUrl}/posts`)
      .then(res => { this.posts = res.data })
      .catch(err => { this.posts = [{ subject: 'Error', entry: 'There was an error: ' + err.message, mood: 'sad', id: -1 }] })
  }
}
</script>

<template>
  <div class="container py-4">
    <h2 class="mb-4">Blog Posts</h2>

    <div v-if="posts && posts.length">
      <blog-post
        v-for="post in posts"
        :key="post.id ?? post.subject ?? JSON.stringify(post)"
        :subject="post.subject"
        :entry="post.entry"
        :mood="post.mood"
      />
    </div>

    <div v-else>
      <p>No posts available.</p>
    </div>
  </div>
</template>