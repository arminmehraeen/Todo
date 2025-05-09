<template>
  <div class="app-container">
    <Header class="animate__animated animate__fadeIn"></Header>

    <div class="container py-4">
      <div class="row g-4">
        <!-- New Task Form -->
        <div class="col-lg-6 order-lg-2">
          <div class="card shadow-sm animate__animated animate__fadeInRight">
            <div class="card-header bg-primary text-white">
              <h4 class="mb-0">کار جدید</h4>
            </div>
            <div class="card-body">
              <form @submit.prevent="add()" dir="rtl" class="needs-validation" novalidate>
                <div class="mb-3">
                  <label class="form-label">عنوان</label>
                  <input v-model="title" type="text" class="form-control" :class="{'is-invalid': eTitle}">
                  <div class="invalid-feedback">{{ eTitle }}</div>
                </div>

                <div class="mb-3">
                  <label class="form-label">موضوع</label>
                  <input v-model="subject" type="text" class="form-control" :class="{'is-invalid': eSubject}">
                  <div class="invalid-feedback">{{ eSubject }}</div>
                </div>

                <div class="mb-3">
                  <label class="form-label">توضیحات</label>
                  <textarea v-model="body" class="form-control" rows="3" :class="{'is-invalid': eBody}"></textarea>
                  <div class="invalid-feedback">{{ eBody }}</div>
                </div>

                <div class="row">
                  <div class="col-md-6 mb-3">
                    <label class="form-label">اهمیت</label>
                    <select v-model="important" class="form-select">
                      <option v-for="(important, index) in importants" :key="index">{{ important }}</option>
                    </select>
                  </div>
                  <div class="col-md-6 mb-3">
                    <label class="form-label">رنگ</label>
                    <select v-model="color" class="form-select">
                      <option v-for="(color, index) in colors" :key="index">{{ color }}</option>
                    </select>
                  </div>
                </div>

                <button class="btn btn-primary w-100" type="submit">
                  <i class="bi bi-plus-circle me-2"></i>ذخیره کردن
                </button>
              </form>
            </div>
          </div>
        </div>

        <!-- Todo List -->
        <div class="col-lg-6 order-lg-1">
          <div class="card shadow-sm animate__animated animate__fadeInLeft">
            <div class="card-header bg-primary text-white">
              <h4 class="mb-0">لیست کارها</h4>
            </div>
            <div class="card-body">
              <div class="d-flex justify-content-between align-items-center mb-3">
                <span class="badge bg-primary">مجموع کارها: {{ todosLenght }} مورد</span>
                <div class="btn-group">
                  <button @click="visibility='all'" 
                          class="btn btn-outline-primary" 
                          :class="{'active': visibility === 'all'}">همه</button>
                  <button @click="visibility='active'" 
                          class="btn btn-outline-success" 
                          :class="{'active': visibility === 'active'}">فعال</button>
                  <button @click="visibility='completed'" 
                          class="btn btn-outline-dark" 
                          :class="{'active': visibility === 'completed'}">تمام شده</button>
                </div>
              </div>

              <div v-if="todos.length === 0" class="alert alert-info text-center">
                لیست کارهای شما خالی است
              </div>

              <div class="todo-list">
                <Card v-for="todo in filteredTodos" 
                      :key="todo.id" 
                      :todo="todo"
                      @remove="removeTodo"
                      @completed="changeStatus"
                      class="animate__animated animate__fadeIn">
                </Card>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Footer class="animate__animated animate__fadeIn"></Footer>
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Card from "./components/Card";
import Swal from 'sweetalert2';
import 'animate.css';

let filters = {
  all: (items) => items,
  active: (items) => items.filter(item => !item.status),
  completed: (items) => items.filter(item => item.status),
};

export default {
  name: 'App',
  components: {
    Header,
    Footer,
    Card
  },
  computed: {
    todosLenght() {
      return this.todos.length;
    },
    filteredTodos() {
      return filters[this.visibility](this.todos);
    }
  },
  methods: {
    add() {
      this.clearError();
      let isValid = true;

      if (!this.title.trim()) {
        this.eTitle = "این فیلد الزامی است";
        isValid = false;
      }
      if (!this.subject.trim()) {
        this.eSubject = "این فیلد الزامی است";
        isValid = false;
      }
      if (!this.body.trim()) {
        this.eBody = "این فیلد الزامی است";
        isValid = false;
      }

      if (isValid) {
        this.todos.push({
          id: this.todosLenght + 1,
          title: this.title.trim(),
          subject: this.subject.trim(),
          body: this.body.trim(),
          important: this.important,
          color: this.color,
          status: false
        });

        Swal.fire({
          title: 'موفق',
          text: 'کار با موفقیت اضافه شد',
          icon: 'success',
          confirmButtonText: 'بستن',
          timer: 2000,
          timerProgressBar: true
        });

        this.clearForm();
      }
    },
    clearError() {
      this.eTitle = "";
      this.eSubject = "";
      this.eBody = "";
    },
    clearForm() {
      this.title = "";
      this.subject = "";
      this.body = "";
      this.color = "مشکی";
      this.important = "کم";
    },
    changeStatus(todo) {
      todo.status = !todo.status;
    },
    removeTodo(id) {
      Swal.fire({
        title: 'آیا مطمئن هستید؟',
        text: "این عملیات قابل بازگشت نیست!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#3085d6',
        confirmButtonText: 'بله، حذف شود!',
        cancelButtonText: 'انصراف'
      }).then((result) => {
        if (result.isConfirmed) {
          this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1);
          Swal.fire({
            title: 'حذف شد!',
            text: 'کار با موفقیت حذف شد.',
            icon: 'success',
            timer: 2000,
            timerProgressBar: true
          });
        }
      });
    },
  },
  data() {
    return {
      visibility: 'all',
      colors: ['آبی', 'مشکی', 'قرمز'],
      importants: ['کم', 'متوسط', 'زیاد'],
      eTitle: "",
      eBody: "",
      eSubject: "",
      title: "",
      subject: "",
      body: "",
      color: "مشکی",
      important: 'کم',
      todos: [],
    }
  }
}
</script>

<style lang="scss">
@import 'bootstrap/scss/bootstrap';
@import 'bootstrap-icons/font/bootstrap-icons.css';

.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f8f9fa;
}

.card {
  border: none;
  border-radius: 10px;
  transition: transform 0.2s;

  &:hover {
    transform: translateY(-5px);
  }
}

.todo-list {
  max-height: 600px;
  overflow-y: auto;
  padding-right: 5px;

  &::-webkit-scrollbar {
    width: 6px;
  }

  &::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 10px;
  }

  &::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 10px;

    &:hover {
      background: #555;
    }
  }
}

.btn-group {
  .btn {
    border-radius: 20px;
    margin: 0 2px;
  }
}

.form-control, .form-select {
  border-radius: 8px;
  padding: 10px;
  
  &:focus {
    box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.15);
  }
}

.badge {
  padding: 8px 12px;
  border-radius: 20px;
}
</style>
