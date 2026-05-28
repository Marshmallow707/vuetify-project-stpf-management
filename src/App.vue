<template>
  <div class="app">

    <!-- GOOGLE FONT -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700;900&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet" />

    <!-- HERO BANNER -->
    <div class="hero">
      <img
        src="https://images.unsplash.com/photo-1541339907198-e08756dedf3f?w=1400&q=80"
        class="hero-img"
        alt="banner"
      />
      <div class="hero-overlay">
        <span class="hero-tag">🎓 IT110 — Web Systems & Technologies</span>
        <h1 class="hero-title">Student Profile<br/>Manager</h1>
        <p class="hero-by">by Mary Adrianne Bisoy</p>
      </div>
    </div>

    <!-- MAIN CONTENT -->
    <div class="main">

      <!-- FORM CARD -->
      <div class="card form-card">
        <div class="card-title">Add New Student</div>

        <label class="field-label">Full Name</label>
        <input
          v-model="name"
          class="field-input"
          placeholder="e.g. Mary Adrianne Bisoy"
        />

        <label class="field-label">Course</label>
        <input
          v-model="course"
          class="field-input"
          placeholder="e.g. BS Information Science"
        />

        <label class="field-label">
          Profile Photo
          <span class="field-optional">— upload or paste URL</span>
        </label>

        <!-- UPLOAD BUTTON -->
        <div class="upload-row">
          <label class="upload-btn">
            📁 Upload Photo
            <input
              type="file"
              accept="image/*"
              style="display:none"
              @change="handleUpload"
            />
          </label>
          <span class="upload-or">or</span>
          <input
            v-model="photo"
            class="field-input url-input"
            placeholder="Paste image URL here"
          />
        </div>

        <!-- PHOTO PREVIEW -->
        <div v-if="previewPhoto" class="preview-row">
          <img :src="previewPhoto" class="preview-img" alt="preview" />
          <div>
            <div class="preview-label">Photo Preview</div>
            <button class="clear-btn" @click="clearPhoto">✕ Clear</button>
          </div>
        </div>

        <p v-if="showError" class="error-msg">⚠️ Name and course are required!</p>

        <button class="add-btn" @click="addStudent">
          Add Student ➕
        </button>
      </div>

      <!-- DIVIDER -->
      <div class="divider">
        <span v-if="students.length > 0">
          {{ students.length }} student{{ students.length > 1 ? 's' : '' }} enrolled
        </span>
        <span v-else>No students yet</span>
      </div>

      <!-- EMPTY STATE -->
      <div v-if="students.length === 0" class="empty-state">
        <div class="empty-icon">🫙</div>
        <p>Add your first student above!</p>
      </div>

      <!-- STUDENT LIST -->
      <StudentCard
        v-for="(student, index) in students"
        :key="index"
        :name="student.name"
        :course="student.course"
        :photo="student.photo"
        @remove="removeStudent(index)"
      />

    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import StudentCard from './components/StudentCard.vue'

const name = ref('')
const course = ref('')
const photo = ref('')
const uploadedPhoto = ref('')
const showError = ref(false)

const previewPhoto = computed(() => uploadedPhoto.value || photo.value || '')

function handleUpload(e) {
  const file = e.target.files[0]
  if (!file) return
  const reader = new FileReader()
  reader.onload = (ev) => {
    uploadedPhoto.value = ev.target.result
    photo.value = ''
  }
  reader.readAsDataURL(file)
}

function clearPhoto() {
  photo.value = ''
  uploadedPhoto.value = ''
}

const students = ref([
  {
    name: 'Mary Adrianne Bisoy',
    course: 'BS Information Science',
    photo: 'https://api.dicebear.com/7.x/thumbs/svg?seed=Mary'
  },
  {
    name: 'Juan Dela Cruz',
    course: 'BS Computer Science',
    photo: 'https://api.dicebear.com/7.x/thumbs/svg?seed=Juan'
  }
])

function addStudent() {
  if (name.value.trim() === '' || course.value.trim() === '') {
    showError.value = true
    return
  }
  showError.value = false
  students.value.push({
    name: name.value,
    course: course.value,
    photo: previewPhoto.value || `https://api.dicebear.com/7.x/thumbs/svg?seed=${name.value}`
  })
  name.value = ''
  course.value = ''
  photo.value = ''
  uploadedPhoto.value = ''
}

function removeStudent(index) {
  students.value.splice(index, 1)
}
</script>

<style>
* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: #0f0a1e;
  font-family: 'DM Sans', sans-serif;
  color: #e2d9f3;
  min-height: 100vh;
}

/* HERO */
.hero {
  position: relative;
  height: 260px;
  overflow: hidden;
}
.hero-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: brightness(0.35) saturate(1.2);
}
.hero-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 20px;
  background: linear-gradient(to bottom, transparent 40%, #0f0a1e);
}
.hero-tag {
  background: #7c3aed33;
  color: #c4b5fd;
  border: 1px solid #7c3aed66;
  border-radius: 999px;
  padding: 4px 14px;
  font-size: 0.78rem;
  font-weight: 600;
  letter-spacing: 1px;
  margin-bottom: 12px;
  display: inline-block;
}
.hero-title {
  font-family: 'Outfit', sans-serif;
  font-size: 2.6rem;
  font-weight: 900;
  color: white;
  line-height: 1.1;
  margin-bottom: 8px;
}
.hero-by {
  color: #9d7ecc;
  font-size: 0.9rem;
}

/* MAIN */
.main {
  max-width: 600px;
  margin: 0 auto;
  padding: 32px 20px 60px;
}

/* CARD */
.card {
  background: #1a1035;
  border: 1px solid #6d3fcf33;
  border-radius: 20px;
  padding: 28px;
  margin-bottom: 24px;
}
.card-title {
  font-family: 'Outfit', sans-serif;
  font-size: 1.1rem;
  font-weight: 700;
  color: #c4b5fd;
  margin-bottom: 20px;
  padding-bottom: 12px;
  border-bottom: 1px solid #6d3fcf22;
}

/* FIELDS */
.field-label {
  display: block;
  font-size: 0.78rem;
  font-weight: 600;
  color: #9d7ecc;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 6px;
  margin-top: 16px;
}
.field-optional {
  text-transform: none;
  letter-spacing: 0;
  color: #6d4fa0;
  font-weight: 400;
  font-size: 0.75rem;
}
.field-input {
  width: 100%;
  background: #0f0a1e;
  border: 1px solid #6d3fcf44;
  border-radius: 10px;
  padding: 10px 14px;
  color: #e2d9f3;
  font-family: 'DM Sans', sans-serif;
  font-size: 0.9rem;
  outline: none;
  transition: border-color 0.2s;
}
.field-input:focus {
  border-color: #7c3aed;
}
.field-input::placeholder {
  color: #4a3870;
}

/* UPLOAD */
.upload-row {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 4px;
}
.upload-btn {
  background: #2d1f4e;
  border: 1px solid #6d3fcf66;
  border-radius: 10px;
  padding: 9px 16px;
  color: #c4b5fd;
  font-size: 0.85rem;
  cursor: pointer;
  white-space: nowrap;
  transition: all 0.2s;
  font-family: 'DM Sans', sans-serif;
}
.upload-btn:hover {
  background: #3d2a6e;
  border-color: #a78bfa;
}
.upload-or {
  color: #4a3870;
  font-size: 0.8rem;
  flex-shrink: 0;
}
.url-input {
  flex: 1;
  min-width: 120px;
}

/* PREVIEW */
.preview-row {
  display: flex;
  align-items: center;
  gap: 14px;
  background: #0f0a1e;
  border: 1px solid #6d3fcf33;
  border-radius: 12px;
  padding: 12px 16px;
  margin-top: 12px;
}
.preview-img {
  width: 52px;
  height: 52px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #7c3aed;
}
.preview-label {
  font-size: 0.8rem;
  color: #9d7ecc;
  margin-bottom: 4px;
}
.clear-btn {
  background: none;
  border: 1px solid #6d3fcf44;
  color: #9d7ecc;
  border-radius: 6px;
  padding: 3px 10px;
  font-size: 0.75rem;
  cursor: pointer;
  transition: all 0.2s;
  font-family: 'DM Sans', sans-serif;
}
.clear-btn:hover {
  border-color: #f0abfc;
  color: #f0abfc;
}

/* ADD BUTTON */
.add-btn {
  width: 100%;
  background: linear-gradient(135deg, #7c3aed, #a855f7);
  border: none;
  border-radius: 12px;
  padding: 12px;
  color: white;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 0.95rem;
  cursor: pointer;
  margin-top: 20px;
  transition: opacity 0.2s, transform 0.2s;
}
.add-btn:hover {
  opacity: 0.9;
  transform: translateY(-1px);
}
.error-msg {
  color: #f87171;
  font-size: 0.82rem;
  margin-top: 8px;
  text-align: center;
}

/* DIVIDER */
.divider {
  text-align: center;
  color: #6d4fa0;
  font-size: 0.82rem;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 20px;
  position: relative;
}
.divider::before, .divider::after {
  content: '';
  position: absolute;
  top: 50%;
  width: 28%;
  height: 1px;
  background: #6d3fcf22;
}
.divider::before { left: 0; }
.divider::after { right: 0; }

/* EMPTY */
.empty-state {
  text-align: center;
  padding: 40px 0;
  color: #4a3870;
}
.empty-icon { font-size: 2.5rem; margin-bottom: 8px; }

#app { min-height: 100vh; }
</style>