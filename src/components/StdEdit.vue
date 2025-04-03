<template>
  <div>
    <form @submit.prevent="saveEdit">
      <div class="mb-3">
        <label class="form-label">รหัสวิชา</label>
        <input type="text" class="form-control" v-model="subjectData.id" disabled />
      </div>
      <div class="mb-3">
        <label class="form-label">ชื่อวิชา</label>
        <input type="text" class="form-control" v-model="subjectData.name" required />
      </div>
      <div class="mb-3">
        <label class="form-label">เกรด</label>
        <input type="text" class="form-control" v-model="subjectData.grade" required />
      </div>
      <div class="mb-3">
        <label class="form-label">หน่วยกิต</label>
        <input type="number" class="form-control" v-model="subjectData.credit" required />
      </div>
      <button type="submit" class="btn btn-primary">บันทึก</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ["subject"],
  data() {
    return {
      subjectData: { ...this.subject },
    };
  },
  methods: {
    saveEdit() {
        fetch(`http://localhost:3000/students/${subjectData.id}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(subjectData),
        })
        
        this.$emit("updateSubject", this.subjectData);
    },
  },
};
</script>
