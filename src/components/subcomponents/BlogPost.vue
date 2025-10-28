<script>
export default {
  props: {
    subject: { type: String, required: true },
    entry:   { type: String, required: true },
    mood:    { type: String, required: false, default: 'neutral' }
  },
  computed: {
    moodImage() {
      // Map mood → image path from /public/assets
      const map = {
        happy: '/assets/happy.png',
        sad: '/assets/sad.png',
        neutral: '/assets/neutral.png',
        angry: '/assets/angry.png'
      }
      return map[this.mood?.toLowerCase?.()] || map.neutral
    }
  }
}
</script>

<template>
  <div class="card mb-3 shadow-sm" style="max-width: 640px;">
    <div class="row g-0">
      <div class="col-md-4 d-flex align-items-center justify-content-center p-2">
        <img :src="moodImage" :alt="mood" class="img-fluid rounded-start" />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{ subject }}</h5>

          <!-- IMPORTANT: Only one <p> in the card so the test can target it -->
          <p class="card-text">{{ entry }}</p>

          <!-- Mood rendered without creating a second <p> -->
          <small class="text-muted d-block">Mood: {{ mood }}</small>
        </div>
      </div>
    </div>
  </div>
</template>