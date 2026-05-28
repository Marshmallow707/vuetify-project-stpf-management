<template>
  <div class="s-card">
    <div class="s-avatar-wrap">
      <img :src="photo || defaultAvatar" class="s-avatar" :alt="name" />
    </div>
    <div class="s-info">
      <div class="s-name">{{ name }}</div>
      <div class="s-course">{{ course }}</div>
    </div>
    <button class="s-remove" @click="$emit('remove')" title="Remove student">
      ✕
    </button>
  </div>
</template>

<script setup>
defineProps({ name: String, course: String, photo: String })
defineEmits(['remove'])
const defaultAvatar = 'https://api.dicebear.com/7.x/thumbs/svg?seed='
</script>

<style scoped>
.s-card {
  display: flex;
  align-items: center;
  gap: 16px;
  background: #1e1535;
  border: 1px solid #6d3fcf44;
  border-radius: 16px;
  padding: 16px 20px;
  max-width: 560px;
  margin: 0 auto 12px;
  transition: border-color 0.2s, transform 0.2s;
}
.s-card:hover {
  border-color: #a78bfa;
  transform: translateY(-2px);
}
.s-avatar-wrap {
  flex-shrink: 0;
}
.s-avatar {
  width: 52px;
  height: 52px;
  border-radius: 50%;
  border: 2px solid #7c3aed;
  object-fit: cover;
  background: #2d1f4e;
}
.s-info {
  flex: 1;
}
.s-name {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 1rem;
  color: #e2d9f3;
}
.s-course {
  font-size: 0.82rem;
  color: #9d7ecc;
  margin-top: 2px;
}
.s-remove {
  background: none;
  border: 1px solid #6d3fcf44;
  color: #9d7ecc;
  width: 32px;
  height: 32px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.s-remove:hover {
  background: #7c3aed22;
  border-color: #a78bfa;
  color: #f0abfc;
}
</style>