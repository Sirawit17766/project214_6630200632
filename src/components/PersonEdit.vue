<template>
  <div>
    <h3 class="text-primary">แก้ไขข้อมูลส่วนตัว</h3>
    <form @submit.prevent="saveChanges">
      <div class="mb-3">
        <label class="form-label">ชื่อ</label>
        <input type="text" class="form-control" v-model="localInfor.name" />
      </div>
      <div class="mb-3">
        <label class="form-label">รหัสนิสิต</label>
        <input type="text" class="form-control" v-model="localInfor.student_id" />
      </div>
      <div class="mb-3">
        <label class="form-label">สาขา</label>
        <input type="text" class="form-control" v-model="localInfor.major" />
      </div>
      <div class="mb-3">
        <label class="form-label">รหัสสาขา</label>
        <input type="text" class="form-control" v-model="localInfor.major_id" />
      </div>
      <div class="mb-3">
        <label class="form-label">โรงเรียนที่เคยศึกษา</label>
        <input type="text" class="form-control" v-model="localInfor.school" />
      </div>
      <button type="submit" class="btn btn-success">บันทึก</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ["infor"],
  data() {
    return {
      localInfor: { ...this.infor },
    };
  },
  methods: {
    async saveChanges() {
      const res = await fetch("http://localhost:3000/information", {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.localInfor),
      })
      this.$emit("updateInfo", this.localInfor);
    },
  },
};
</script>
