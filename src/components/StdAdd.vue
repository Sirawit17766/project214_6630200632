<template>
    <div class="card" style="background-color: #fffdf0">
      <div class="card-body">
        <form @submit.prevent="handleSubmit()">
          <div class="row">
            <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
              <label for="stdId" class="form-label">รหัสวิชา</label>
              <input
                type="text"
                id="stdId"
                class="form-control"
                v-model.trim="std.id"
              />
            </div>
            <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
              <label for="stdName" class="form-label">ชื่อวิชา</label>
              <input
                type="text"
                id="stdName"
                class="form-control"
                v-model.trim="std.name"
              />
            </div>
  
            <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
              <label for="stdYr" class="form-label">เกรด</label>
              <select id="stdYr" class="form-select" v-model="std.grade">
                <option value="A">A</option>
                <option value="B+">B+</option>
                <option value="B">B</option>
                <option value="C+">C+</option>
                <option value="C">C</option>
                <option value="D+">D+</option>
                <option value="D">D</option>
                <option value="F">F</option>
  
              </select>
            </div>
            <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
              <label for="stdYr" class="form-label">หน่วยกิต</label>
              <select id="stdYr" class="form-select" v-model="std.credit">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
                <option value="6">6</option>
              </select>
            </div>
            <div class="col-2 mt-4 d-flex algin-item-center justify-content-center ">
              <button type="submit" class="btn btn-warning ">บันทึก</button>
            </div>
          </div>
          <div class="alert alert-success mt-2" v-if="addOK">
            {{ addMessage }}
          </div>
          <div class="alert alert-danger mt-2" v-if="addErr">
            {{ addMessage }}
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  import { EventBus } from "../event-bus";
  export default {
    name: "StdAdd.vue",
    data() {
      return {
        std: {
          id: null,
          name: null,
          grade: null,
          credit: null,
          isActive: false,
        },
        addOK: false,
        addErr: false,
        addMessage: null,
      };
    },
    methods: {
      handleSubmit() {
        let student = {
          id: this.std.id,
          name: this.std.name,
          grade: this.std.grade,
          credit: this.std.credit,
          isActive: false,
        };
        
        fetch(`http://localhost:3000/students`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(student),
        })
          .then(() => {
            this.addOK = true;
            this.addErr = false;
            this.addMessage = "บันทึกข้อมูลสำเร็จ";
            EventBus.emit("stdChange", { message: "add" });
          })
          .catch((err) => {
            this.addErr = true;
            this.addOK = false;
            this.addMessage = err;
          });
      },
    },
  };
  </script>
  
  <style scoped>
  body {
    margin: 0;
    padding: 0;
    width: 100vw;
    overflow-x: hidden;
  }
  </style>