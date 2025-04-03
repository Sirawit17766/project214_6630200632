<template>
  <div>
    <h3 class="text-primary">รายวิชาที่ศึกษา</h3>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>รหัสวิชา</th>
          <th>ชื่อวิชา</th>
          <th>เกรด</th>
          <th>หน่วยกิต</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="subject in subjects" :key="subject.id">
          <td>{{ subject.id }}</td>
          <td>{{ subject.name }}</td>
          <td>{{ subject.grade }}</td>
          <td>{{ subject.credit }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      subjects: [],
    };
  },
  mounted() {
    fetch("http://localhost:3000/students")
      .then((res) => res.json())
      .then((data) => {
        this.subjects = data;
      })
      .catch((err) => console.error("Error fetching subjects:", err));
  },
};
</script>
