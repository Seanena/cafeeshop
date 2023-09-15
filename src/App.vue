<template>
  <div id="app" class="container my-5">
      <h1 class="mb-4 text-center">จองโต๊ะร้านกาแฟ</h1>
    <!-- แสดงรายชื่อร้าน -->
    <div class="row">
      <div v-for="cafe in cafes" :key="cafe.id" class="col-md-6 mb-3">
        <div class="card">
          <img :src="cafe.image" class="card-img-top" alt="Cafe Image">
          <div class="card-body">
            <h2 class="card-title">{{ cafe.name }}</h2>
            <p class="card-text" v-if="cafe.tables > 0">ว่าง: {{ cafe.tables }} โต๊ะ</p>
            <p class="card-text" v-else>เต็ม</p>
            <button v-if="cafe.tables > 0" @click="showReservationForm(cafe.id)" class="btn btn-primary">จอง</button>
          </div>
        </div>
      </div>
    </div>

    <!-- แสดงฟอร์มการจอง -->
    <div v-if="showForm" class="card my-4">
      <div class="card-body">
        <h2 class="card-title">กรอกข้อมูล</h2>
        <form @submit.prevent="makeReservation">
          <div class="mb-3">
            <label for="name" class="form-label">ชื่อคนจอง:</label>
            <input v-model="reservation.name" type="text" class="form-control" required>
          </div>
          <div class="mb-3">
            <label for="phone" class="form-label">เบอร์โทร:</label>
            <input v-model="reservation.phone" type="tel" class="form-control" pattern="[0-9]{10}" required @input="handlePhoneInput">
            <small class="text-muted">กรุณากรอกเบอร์โทร 10 หลัก</small>
          </div>
          <div class="mb-3">
            <label for="date" class="form-label">วัน/เดือน/ปี:</label>
            <input v-model="reservation.date" type="date" class="form-control" required>
          </div>
          <div class="mb-3">
            <label for="time" class="form-label">เวลา:</label>
            <input v-model="reservation.time" type="time" class="form-control" required>
          </div>
          <div class="mb-3">
            <label for="tables" class="form-label">เลขโต๊ะ:</label>
            <input v-model="reservation.tables" type="number" class="form-control" min="1" required>
          </div>
          <button type="submit" class="btn btn-primary">จองโต๊ะ</button>
        </form>
      </div>
    </div>

    <!-- แสดงรายละเอียดการจอง -->
    <div v-if="showDetails" class="card my-4">
      <div class="card-body">
        <h2 class="card-title">ลายละเอียดข้อมูล</h2>
        <p class="card-text">ชื่อคนจอง: {{ reservation.name }}</p>
        <p class="card-text">เบอร์โทร: {{ reservation.phone }}</p>
        <p class="card-text">วัน/เดือน/ปี: {{ reservation.date }}</p>
        <p class="card-text">เวลา: {{ reservation.time }}</p>
        <p class="card-text">เลขโต๊ะ: {{ reservation.tables }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cafes: [
        { id: 1, name: 'Caffee Wing', tables: 4, image: '/src/assets/img/coffee-wing.jpeg' },
        { id: 2, name: 'Caffee Stand', tables: 2, image: '/src/assets/img/coffee-02.jpeg' }
      ],
      showForm: false,
      showDetails: false,
      reservation: {
        name: '',
        phone: '',
        date: '',
        time: '',
        tables: 1
      }
    };
  },
  methods: {
    showReservationForm(cafeId) {
      this.showForm = true;
      this.reservation.cafeId = cafeId;
    },
    makeReservation() {
      // สามารถทำการจองโต๊ะที่นี้
      this.showForm = false;
      this.showDetails = true;

      this.cafes.forEach(cafe => {
        if (cafe.id === this.reservation.cafeId) {
          cafe.tables -= this.reservation.tables;
        }
      });
    },
    handlePhoneInput() {
      this.reservation.phone = this.reservation.phone.replace(/\D/g, ''); // ลบทุกอักขระที่ไม่ใช่ตัวเลข
    }
  }
};
</script>

<style>
/* เพิ่ม CSS ของ Bootstrap */
</style>