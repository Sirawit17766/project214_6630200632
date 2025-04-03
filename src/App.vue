<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top">
      <div class="container">
        <a class="navbar-brand fw-bold" style="color: #a855f7;" href="#front">HOME</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav ms-auto">
            <a class="nav-link fw-bold text-uppercase" style="color: #4a4a7a;" href="#profile">PROFILE</a>
            <a class="nav-link fw-bold text-uppercase" style="color: #4a4a7a;" href="#education">EDUCATION</a>
            <a class="nav-link fw-bold text-uppercase" style="color: #4a4a7a;" href="#contact">CONTACT</a>
          </div>
        </div>
      </div>
    </nav>

    <div style="height: 60px;"></div>

    <section id="front" class="py-5 min-vh-100 d-flex align-items-center wallpaper">
      <div class="container d-flex justify-content-center align-items-center">
        <div class="row text-center flex-column">
          <h1 class="display-2 fw-bold text-primary">My Portfolio</h1>
          <h2 class="mt-4 lead text-dark">By Mr. Sirawit Tathip</h2>
        </div>
      </div>
    </section>
    <section class="hero-section d-flex align-items-center min-vh-100 bg-light border p-5" id="profile" style="border: 2px solid #ccc; border-radius: 10px;">
        <div class="container p-5">
            <div class="row align-items-center justify-content-center">
                <div class="col-lg-6 text-lg-start text-center">
                    <h1 class="display-3 fw-bold text-primary mb-4">PROFILE</h1>
                    <div class="mb-3">
                        <h4 class="fw-bold text-dark">ชื่อ: <span class="fw-normal">{{ infor.name }}</span></h4>
                        <h4 class="fw-bold text-dark">รหัสนิสิต: <span class="fw-normal">{{ infor.student_id }}</span></h4>
                        <h4 class="fw-bold text-dark">สาขา: <span class="fw-normal">{{ infor.major }}</span></h4>
                        <h4 class="fw-bold text-dark">รหัสสาขา: <span class="fw-normal">{{ infor.major_id }}</span></h4>
                        <h4 class="fw-bold text-dark">โรงเรียนที่เคยศึกษา: <span class="fw-normal">{{ infor.school }}</span></h4>
                    </div>
                    <p class="mt-4 lead text-muted">By Mr. Sirawit Tathip</p>
                    <button type="button" class="btn btn-primary mt-3" @click="showPersonEdit = !showPersonEdit">
                        <i class="bi bi-pencil-square me-2"></i> แก้ไขรายละเอียด
                    </button>
                    <div v-if="showPersonEdit" class="mt-3">
                        <PersonEdit :infor="infor" @updateInfo="updateInfo" />
                    </div>
                </div>
                <div class="col-lg-6 text-lg-end text-center">
                    <img src="/pic/1.gif" alt="Profile" class="img-thumbnail rounded-circle shadow-lg" style="width: 500px; height: 500px; object-fit: cover;" />
                </div>
            </div>
        </div>
    </section>

    <section id="education" class="py-5 min-vh-100 bg-light">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-10">
            <div class="card shadow-lg border-0 rounded-4">
              <div class="card-header bg-primary text-white text-center py-3 rounded-top-4">
                <h3 class="mb-0">Education</h3>
              </div>
              <div class="card-body p-4 bg-white">
                <p class="text-muted text-center">รายละเอียดรายวิชาและเกรดที่ได้รับ</p>
                <div class="table-responsive">
                  <table class="table table-striped">
                    <thead>
                      <tr>
                        <th>รหัสวิชา</th>
                        <th>ชื่อวิชา</th>
                        <th>เกรด</th>
                        <th>หน่วยกิต</th>
                        <th></th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(subject, index) in subjects" :key="index">
                        <td>{{ subject.id }}</td>
                        <td>{{ subject.name }}</td>
                        <td>{{ subject.grade }}</td>
                        <td>{{ subject.credit }}</td>
                        <td class="text-end">
                          <button type="button" class="btn btn-sm btn-warning me-2" @click="editSubject(index)">
                            <i class="bi bi-pencil-fill"></i> แก้ไข
                          </button>
                          <button type="button" class="btn btn-sm btn-danger me-2" @click="removeSubject(index)">
                            <i class="bi bi-pencil-fill"></i> ลบ
                          </button>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="card-footer bg-white py-3 d-flex justify-content-end rounded-bottom-4">
                <button type="button" class="btn btn-success" @click="showAdd = !showAdd">
                  <i class="bi bi-plus-circle me-2"></i> เพิ่มวิชา
                </button>
              </div>
            </div>
          </div>
          <transition name="slide-fade">
            <div class="col-lg-8 mt-4" v-if="showAdd || editingIndex !== null">
              <div class="card shadow border-0 rounded-4">
                <div class="card-header" :class="{'bg-secondary text-white py-3 rounded-top-4': showAdd, 'bg-warning text-dark py-3 rounded-top-4': editingIndex !== null}">
                  <h5 class="mb-0">{{ showAdd ? 'เพิ่มรายวิชา' : 'แก้ไขรายวิชา' }}</h5>
                </div>
                <div class="card-body p-4 bg-white">
                  <form @submit.prevent="saveSubject">
                    <div class="mb-3">
                      <label for="code" class="form-label">รหัสวิชา</label>
                      <input type="text" class="form-control" id="code" v-model="currentSubject.id" required>
                    </div>
                    <div class="mb-3">
                      <label for="name" class="form-label">ชื่อวิชา</label>
                      <input type="text" class="form-control" id="name" v-model="currentSubject.name" required>
                    </div>
                    <div class="mb-3">
                      <label for="grade" class="form-label">เกรด</label>
                      <select class="form-select" id="grade" v-model="currentSubject.grade" required>
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
                    <div class="mb-3">
                      <label for="credit" class="form-label">หน่วยกิต</label>
                      <input type="number" class="form-control" id="credit" v-model="currentSubject.credit" required min="1" max="4">
                    </div>
                    <div class="d-flex justify-content-end">
                      <button type="button" class="btn btn-secondary me-2" @click="cancelAddEdit">
                        ยกเลิก
                      </button>
                      <button type="submit" class="btn" :class="{'btn-success': showAdd, 'btn-warning': editingIndex !== null}">
                        {{ showAdd ? 'เพิ่ม' : 'บันทึก' }}
                      </button>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </section>
    <section id="contact" class="py-5 min-vh-100 bg-light">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-8">
            <div class="contact-card">
              <div class="contact-info">
                <h2 class="fw-bold text-primary mb-4">Contact Me</h2>
                <p class="lead text-muted mb-5">
                  Feel free to reach out if you have any questions or opportunities. I'm always open to connecting!
                </p>
              </div>
              <div class="contact-details">
                <div class="contact-item">
                  <i class="bi bi-github text-primary contact-icon"></i>
                  <div>
                    <h6 class="mb-0 fw-bold text-dark">GitHub</h6>
                    <a href="https://github.com/yourusername" target="_blank" class="text-muted">github.com/yourusername</a>
                  </div>
                </div>
                <div class="contact-links-row">
                  <div class="contact-item">
                    <i class="bi bi-envelope-fill text-primary contact-icon"></i>
                    <div>
                      <h6 class="mb-0 fw-bold text-dark">Email</h6>
                      <a href="mailto:sirawit.ta@ku.th" class="text-muted">sirawit.ta@ku.th</a>
                    </div>
                  </div>
                  <div class="contact-item">
                    <i class="bi bi-phone-fill text-primary contact-icon"></i>
                    <div>
                      <h6 class="mb-0 fw-bold text-dark">Phone</h6>
                      <p class="text-muted">0637608530</p>
                    </div>
                  </div>
                </div>
              </div>
              <hr class="my-4">
              <p class="text-center text-muted mb-0">--------------------------------------</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import PersonEdit from "./components/PersonEdit.vue";

export default {
  components: {
    PersonEdit,
  },
  data() {
    return {
      infor: {
        name: "",
        student_id: "",
        major: "",
        major_id: "",
        school: "",
      },
      subjects: [
        {},
      ],
      showAdd: false,
      showPersonEdit: false,
      currentSubject: {
        id: "",
        name: "",
        grade: "",
        credit: null,
      },
      editingIndex: null,
    };
  },
  async mounted() {
    const res = await fetch("http://localhost:3000/information");

    if (!res.ok) {
      console.log("ไม่สามารถโหลดข้อมูลส่วนตัวได้");
    } else {
      this.infor = await res.json();
    }

    const respone = await fetch("http://localhost:3000/students");

    if (!respone.ok) {
      console.log("ไม่สามารถโหลดข้อมูลวิชาได้");
    } else {
      this.subjects = await respone.json();
    }

  },
  methods: {
    updateInfo(updatedInfo) {
      this.infor = { ...updatedInfo };
    },
    editSubject(index) {
      this.editingIndex = index;
      this.currentSubject = { ...this.subjects[index] };
      this.showAdd = false;
    },
    saveSubject() {
      if (this.editingIndex !== null) {
        fetch(`http://localhost:3000/students/${this.currentSubject.id}`, {
          method: "PUT",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.currentSubject),
        })
        this.subjects[this.editingIndex] = { ...this.currentSubject };
        this.editingIndex = null;
      } else {
        fetch(`http://localhost:3000/students`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.currentSubject),
        })
        this.subjects.push({ ...this.currentSubject });
      }
      this.currentSubject = { id: "", name: "", grade: "", credit: null };
      this.showAdd = false;
    },
    cancelAddEdit() {
      this.showAdd = false;
      this.editingIndex = null;
      this.currentSubject = { id: "", name: "", grade: "", credit: null };
    },
    async removeSubject(index) {
      fetch(`http://localhost:3000/students/${this.subjects[index].id}`, {
        method: "DELETE"
      })

      await new Promise(resolve => setTimeout(resolve, 100));

      const respone = await fetch("http://localhost:3000/students");
      if (!respone.ok) {
        console.log("ไม่สามารถโหลดข้อมูลวิชาได้");
      } else {
        console.log(this.subjects)
        this.subjects = await respone.json();
        console.log(this.subjects)
      }
    }
  },
};
</script>

<style scoped>
.wallpaper {
  background: url('/pic/2.gif') no-repeat center center fixed;
  background-size: cover;
  height: 100vh;
  width: 100%;
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}

.navbar-nav .nav-link {
  color: #4a4a7a;
  font-weight: normal;
  margin-left: 15px;
  font-size: 0.9rem;
}

.navbar-nav .nav-link:hover {
  color: #7b68ee;
}

.navbar-brand {
  color: #a855f7 !important;
  font-weight: normal;
  font-size: 1rem;
}

.navbar-light .navbar-nav .nav-link.active,
.navbar-light .navbar-nav .show > .nav-link {
  color: #4a4a7a;
}

.navbar-light .navbar-toggler {
  border-color: rgba(0, 0, 0, 0.1);
}

.navbar-light .navbar-toggler-icon {
  background-image: url("data:image/svg+xml,%3csvg viewBox='0 0 30 30' xmlns='http://www.w3.org/2000/svg'%3e%3cpath stroke='rgba(0, 0, 0, 0.5)' stroke-linecap='round' stroke-miterlimit='10' stroke-width='2' d='M4 7h22M4 15h22M4 23h22'/%3e%3c/svg%3e");
}

#profile {
  padding-top: 80px;
}

#profile .display-3 {
  font-size: 2.5rem;
}

#profile .img-thumbnail {
  border-width: 5px;
  border-color: #a855f7;
}

#contact {
  padding-top: 80px;
}

#contact .bi {
  font-size: 2rem;
}

#contact a {
  text-decoration: none;
}

#contact a:hover {
  color: #7b68ee;
}
.contact-card {
    border: 2px solid #007bff;
    border-radius: 10px;
    padding: 30px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    background-color: white;
}

.contact-info {
  text-align: center;
}
.contact-info h6,
.contact-info a,
.contact-info p {
  text-align: center;
}

.contact-icon {
    font-size: 2rem;
    margin-right: 15px;
}

.contact-details {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.contact-item {
    display: flex;
    align-items: center;
    margin-bottom: 1.5rem;
    width: 100%;
    justify-content: center;
}

.contact-links-row {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
}

@media (min-width: 768px) {
  .contact-links-row {
      flex-direction: row;
      justify-content: center;
  }

  .contact-item {
      width: auto;
      margin-right: 2rem;
      margin-left: 2rem;
  }
}
</style>