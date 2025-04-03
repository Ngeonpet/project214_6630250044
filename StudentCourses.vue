<template>
  <div class="student-courses">
    <h2>รายละเอียดวิชา</h2>
    <table class="courses-table">
      <thead>
        <tr>
          <th>รหัสวิชา</th>
          <th>ชื่อวิชา</th>
          <th>หน่วยกิต</th>
          <th>เกรด</th>
          <th>จัดการ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(subject, index) in subjectEdits" :key="subject.id">
          <td v-if="editingIndex !== index">{{ subject.code }}</td>
          <td v-else><input v-model="subject.code" required /></td>
          
          <td v-if="editingIndex !== index">{{ subject.name }}</td>
          <td v-else><input v-model="subject.name" required /></td>
          
          <td v-if="editingIndex !== index">{{ subject.credits }}</td>
          <td v-else><input v-model="subject.credits" type="number" required /></td>
          
          <td v-if="editingIndex !== index">{{ subject.grade }}</td>
          <td v-else><input v-model="subject.grade" required /></td>
          
          <td>
            <button class="edit-btn" v-if="editingIndex !== index" @click="editSubject(index)">แก้ไข</button>
            <button class="save-btn" v-else @click="saveEdit(index)">บันทึก</button>
            <button class="delete-btn" @click="deleteSubject(index)">ลบ</button>
          </td>
        </tr>
      </tbody>
    </table>
    <h3>เพิ่มวิชาใหม่</h3>
    <form @submit.prevent="addSubject" class="add-subject-form">
      <div class="form-group">
        <label>รหัสวิชา:</label>
        <input v-model="newSubject.code" required />
      </div>
      <div class="form-group">
        <label>ชื่อวิชา:</label>
        <input v-model="newSubject.name" required />
      </div>
      <div class="form-group">
        <label>หน่วยกิต:</label>
        <input v-model="newSubject.credits" type="number" required />
      </div>
      <div class="form-group">
        <label>เกรด:</label>
        <input v-model="newSubject.grade" required />
      </div>
      <button type="submit" class="add-btn">เพิ่มวิชา</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "StudentCourses",
  props: ['subjects'],
  data() {
    return {
      editingIndex: null,
      newSubject: { code: '', name: '', credits: '', grade: '' },
      subjectEdits: this.subjects.map(subject => ({ ...subject }))
    }
  },
  watch: {
    subjects(newVal) {
      this.subjectEdits = newVal.map(subject => ({ ...subject }))
    }
  },
  methods: {
    editSubject(index) {
      this.editingIndex = index;
    },
    saveEdit(index) {
      this.editingIndex = null;
      this.$emit('update-subjects', this.subjectEdits);
    },
    deleteSubject(index) {
      this.subjectEdits.splice(index, 1);
      this.$emit('update-subjects', this.subjectEdits);
    },
    addSubject() {
      this.subjectEdits.push({ ...this.newSubject });
      this.newSubject = { code: '', name: '', credits: '', grade: '' };
      this.$emit('update-subjects', this.subjectEdits);
    }
  }
}
</script>

<style scoped>
.student-courses {
  text-align: center;
  margin-bottom: 20px;
}
.courses-table {
  width: 100%;
  margin: 0 auto 20px;
  border-collapse: collapse;
}
.courses-table th,
.courses-table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}
.courses-table th {
  background-color: #f2f2f2;
}
.edit-btn, .save-btn, .delete-btn, .add-btn {
  background-color: #4CAF50;
  color: #fff;
  border: none;
  padding: 6px 10px;
  border-radius: 5px;
  cursor: pointer;
  margin: 2px;
}
.delete-btn {
  background-color: #f44336;
}
.edit-btn:hover, .save-btn:hover, .delete-btn:hover, .add-btn:hover {
  opacity: 0.9;
}
.add-subject-form {
  max-width: 400px;
  margin: 0 auto;
  text-align: left;
}
.form-group {
  margin-bottom: 10px;
  text-align: left;
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
