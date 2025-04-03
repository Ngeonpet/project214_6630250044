<template>
  <div class="app-container">
    <!-- ส่งข้อมูลนิสิตไปยังคอมโพเนนต์ StudentProfile -->
    <StudentProfile
      :student="student"
      @update-student="updateStudent"
    />
    <hr />
    <!-- ส่งข้อมูลวิชาไปยังคอมโพเนนต์ StudentCourses -->
    <StudentCourses
      :subjects="subjects"
      @update-subjects="updateSubjects"
    />
  </div>
</template>

<script>
import axios from 'axios'
import StudentProfile from './components/StudentProfile.vue'
import StudentCourses from './components/StudentCourses.vue'

export default {
  name: "App",
  components: {
    StudentProfile,
    StudentCourses
  },
  data() {
    return {
      student: {},
      subjects: []
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    // ดึงข้อมูลจาก JSON Server เมื่อแอปเริ่มทำงาน
    fetchData() {
      axios.get('http://localhost:3000/students')
        .then(response => {
          if (response.data && response.data.length > 0) {
            this.student = response.data[0]
          }
        })
        .catch(error => {
          console.error("Error fetching student data:", error)
        })

      axios.get('http://localhost:3000/subjects')
        .then(response => {
          this.subjects = response.data
        })
        .catch(error => {
          console.error("Error fetching subjects data:", error)
        })
    },
    updateStudent(updatedStudent) {
      axios.put(`http://localhost:3000/students/${updatedStudent.id}`, updatedStudent)
        .then(response => {
          this.student = response.data
        })
        .catch(error => {
          console.error("Error updating student:", error)
        })
    },
    updateSubjects(updatedSubjects) {
      // อัปเดตแต่ละวิชาแยกกัน เนื่องจาก JSON Server ไม่รองรับอัปเดตหลายรายการพร้อมกัน
      updatedSubjects.forEach(subject => {
        axios.put(`http://localhost:3000/subjects/${subject.id}`, subject)
          .then(response => {
            console.log(`Updated subject ${subject.id}`)
          })
          .catch(error => {
            console.error(`Error updating subject ${subject.id}:`, error)
          })
      })
      this.subjects = updatedSubjects
    }
  }
}
</script>

<style>
.app-container {
  max-width: 450px;  /* ลดขนาด container ลงครึ่งหนึ่ง */
  width: 100%;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #fff;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  border-radius: 8px;
}
hr {
  margin: 40px 0;
  border: none;
  border-top: 1px solid #ddd;
  width: 100%;
}
</style>
