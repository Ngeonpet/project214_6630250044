<template>
  <div class="student-profile">
    <h2>ข้อมูลนิสิต</h2>
    <div class="profile-card">
      <img :src="student.image" alt="Student Photo" class="profile-image" />
      <div class="profile-details">
      

        <p><strong>ชื่อ:</strong> {{ student.name }}</p>
        <p><strong>รหัสนิสิต:</strong> {{ student.student_id }}</p>
        <p><strong>สาขา:</strong> {{ student.major }}</p>
        <p><strong>โรงเรียน:</strong> {{ student.school }}</p>
      </div>
    </div>
    <button class="edit-btn" @click="toggleEdit">
      {{ editMode ? 'ยกเลิก' : 'แก้ไข' }}
    </button>
    <div v-if="editMode" class="edit-form">
      <h3>แก้ไขข้อมูลนิสิต</h3>
      <form @submit.prevent="save">
        <div class="form-group">
          <label>ชื่อ:</label>
          <input v-model="editableStudent.name" required />
        </div>
        <div class="form-group">
          <label>รหัสนิสิต:</label>
          <input v-model="editableStudent.student_id" required />
        </div>
        <div class="form-group">
          <label>สาขา:</label>
          <input v-model="editableStudent.major" required />
        </div>
        <div class="form-group">
          <label>โรงเรียน:</label>
          <input v-model="editableStudent.school" required />
        </div>
        <div class="form-group">
          <label>URL รูป:</label>
          <input v-model="editableStudent.image" required />
        </div>
        
        <button type="submit" class="save-btn">บันทึก</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "StudentProfile",
  props: ['student'],
  data() {
    return {
      editMode: false,
      editableStudent: JSON.parse(JSON.stringify(this.student))
    }
  },
  watch: {
    student(newVal) {
      this.editableStudent = JSON.parse(JSON.stringify(newVal))
    }
  },
  methods: {
    toggleEdit() {
      this.editMode = !this.editMode;
      this.editableStudent = JSON.parse(JSON.stringify(this.student));
    },
    save() {
      this.$emit('update-student', this.editableStudent);
      this.editMode = false;
    }
  }
}
</script>

<style scoped>
.student-profile {
  text-align: center;
  margin-bottom: 20px;
}
.profile-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 15px;
}
.profile-image {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #ccc;
  margin-bottom: 10px;
}
.profile-details p {
  margin: 5px 0;
}
.edit-btn, .save-btn {
  background-color: #4CAF50;
  color: #fff;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}
.edit-btn:hover, .save-btn:hover {
  background-color: #45a049;
}
.edit-form {
  margin-top: 20px;
  text-align: left;
}
.form-group {
  margin-bottom: 10px;
}
.form-group label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}
.form-group input {
  width: 100%;
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 4px;
}



</style>
