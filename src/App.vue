<template>
  <div>
    <h1>Borrower Register Form</h1>
    
    <form @submit.prevent="submitForm">
      <h2>Personal Information</h2>
      <label for="first_name">First Name</label>
      <input type="text" id="first_name" v-model="first_name" />
      <label for="last_name">Last Name</label>
      <input type="text" id="last_name" v-model="last_name" />

      <h2>Account Information</h2>
      <label for="username">Username</label>
      <input type="text" id="username" v-model="username" />
      <label for="password">Password</label>
      <input type="password" id="password" v-model="password" />
      <label for="email">Email</label>
      <input type="email" id="email" v-model="email" />

      <h2>Profile Details</h2>
      <label for="profile_image">Profile Image</label>
      <input type="file" id="profile_image" @change="handleFileUpload" />
      
      <!-- แสดงรูปภาพที่ถูกเลือก -->
      <div v-if="imageUrl">
        <img :src="imageUrl" alt="Profile Image" width="100" height="100" />
      </div>

      <label for="description">Description</label>
      <textarea id="description" v-model="description"></textarea>

      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

// เก็บค่าข้อมูลฟอร์ม
const first_name = ref('')
const last_name = ref('')
const username = ref('')
const password = ref('')
const email = ref('')
const profile_image = ref(null)
const description = ref('')
const imageUrl = ref(null) // ใช้สำหรับเก็บ URL รูปภาพที่ถูกเลือก

// ฟังก์ชันสำหรับจัดการการอัปโหลดไฟล์
const handleFileUpload = (event) => {
  const file = event.target.files[0]
  profile_image.value = file
  imageUrl.value = URL.createObjectURL(file) // สร้าง URL สำหรับแสดงรูปภาพ
}

// ฟังก์ชันสุดท้ายสำหรับส่งข้อมูลฟอร์มทั้งหมด
const submitForm = async () => {
  const payload = new FormData();
  payload.append('first_name', first_name.value);
  payload.append('last_name', last_name.value);
  payload.append('username', username.value);
  payload.append('password', password.value);
  payload.append('email', email.value);
  payload.append('profile_image', profile_image.value); // เพิ่มไฟล์ที่ถูกเลือก
  payload.append('description', description.value);
  
  // ตรวจสอบข้อมูลที่ส่งไป
  for (let [key, value] of payload.entries()) {
    console.log(`${key}: ${value}`);
  }

  try {
    const response = await axios.post('http://127.0.0.1:8000/borrowers/register/', payload, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    });
    console.log("Form submitted successfully: ", response.data);
  } catch (error) {
    console.error("Error submitting form: ", error.response ? error.response.data : error.message);
  }
}
</script>