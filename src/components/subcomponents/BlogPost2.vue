<script>
export default {
  props: {
    subject: { type: String, required: true },
    entry:   { type: String, required: true },
    mood:    { type: String, default: 'neutral' } // e.g., 'happy', 'sad', 'neutral'
  },
  computed: {
    moodImg() {
      const map = {
        happy: '/assets/happy.png',
        sad: '/assets/sad.png',
        neutral: '/assets/neutral.png'
      }
      return map[this.mood?.toLowerCase()] ?? map.neutral
    }
  }
}
</script>

<template>
  <div class="card h-100 shadow-sm">
    <img :src="moodImg" class="card-img-top" alt="mood image" style="object-fit: contain; max-height: 180px;">
    <div class="card-body">
      <h5 class="card-title mb-2">{{ subject }}</h5>
      <p class="card-text">{{ entry }}</p>
    </div>
    <div class="card-footer bg-transparent d-flex justify-content-end gap-2">
      <slot name="actions"></slot>
    </div>
  </div>
</template>