<template>

  <Header></Header>

  <div class="row">

    <div class="col-md-6 order-md-2 mb-5">
      <h4 class="d-block justify-content-between text-center align-items-center mb-3">
        <span class="text-muted text-center">کار جدید</span>
      </h4>
      <hr>
      <form @submit.prevent="add()" dir="rtl" class="needs-validation" novalidate>

        <div class="row">
          <div class="col-md-6 mb-3">
            <label CLASS="p-2">عنوان</label>
            <input v-model="title" type="text" class="form-control">
            <div v-if="eTitle.length > 0" class="text-danger mt-2">
              {{ eTitle }}
            </div>
          </div>
        </div>

        <div class="mb-3">
          <label class="p-2">موضوع</label>
          <input v-model="subject" type="text" class="form-control">
          <div v-if="eSubject.length > 0" class="text-danger mt-2">
            {{ eSubject }}
          </div>
        </div>

        <div class="mb-3">
          <label class="p-2">توضیحات</label>
          <textarea v-model="body" class="form-control"></textarea>
          <div v-if="eBody.length > 0" class="text-danger mt-2">
            {{ eBody }}
          </div>
        </div>

        <div class="row">
          <div class="col-md-5 mb-3">
            <label class="p-2">اهمیت</label>
            <select v-model="important" class="custom-select p-1 d-block w-100" id="country" required>
              <option v-for="(important,index) in importants" :key="index">{{ important }}</option>
            </select>
            <div class="invalid-feedback">
              Please select a valid country.
            </div>
          </div>
          <div class="col-md-4 mb-3">
            <label class="p-2">رنگ</label>
            <select v-model="color" class="p-1 custom-select d-block w-100" id="state" required>
              <option v-for="(color,index) in colors " :key="index">{{ color }}</option>
            </select>
            <div class="invalid-feedback">
              Please provide a valid state.
            </div>
          </div>
        </div>

        <hr class="mb-4">
        <button class="btn btn-outline-success btn-sm btn-block" type="submit">ذخیره کردن</button>
      </form>
    </div>

    <div class="col-md-6 order-md-1 mb-5">

      <h4 class="d-block justify-content-between text-center align-items-center mb-3">
        <span class="text-muted text-center">لیست کارها</span>
      </h4>
      <hr>

      <ul dir="rtl" class="mb-3">

        <li class="list-group-item d-flex justify-content-between">
          <span>مجموع کارها</span>
          <strong>{{ todosLenght }} مورد</strong>
        </li>
        <hr>

        <li class="list-group-item d-flex justify-content-between">
          <span>نمایش</span>
          <span>
            <button @click="visibility='all'" class="btn btn-sm btn-outline-primary ms-1">همه</button>
            <button @click="visibility='active'" class="btn btn-sm btn-outline-success ms-1">فعال</button>
            <button @click="visibility='completed'" class="btn btn-sm btn-outline-dark">تمام شده</button>
          </span>
        </li>

        <li v-if="todos.length == 0" class="list-group-item mt-2 bg-secondary text-light d-flex justify-content-between">
          لیست کارهای شما خالی است
        </li>

        <Card v-for="todo in filteredTodos" :key="todo.title" :todo="todo"
              v-on:remove="removeTodo($event)" v-on:completed="changeStatus($event)"></Card>

      </ul>

    </div>

  </div>
  <Footer></Footer>

</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Card from "./components/Card";
import Swal from 'sweetalert2';

let filters = {
  all: (items) => {
    return items;
  },
  active: (items) => {
    return items.filter((item) => {
      return !item.status;
    });
  },
  completed: (items) => {
    return items.filter((item) => {
      return item.status;
    });
  },
};

export default {
  name: 'App',
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

      var isOk = true;
      this.clearError();

      let t = this.title.trim();
      if (t == "") {
        this.eTitle = "این فیلد الزامی است";
        isOk = false;
      }
      let s = this.subject.trim();
      if (s == "") {
        this.eSubject = "این فیلد الزامی است";
        isOk = false;
      }
      let b = this.body.trim();
      if (b == "") {
        this.eBody = "این فیلد الزامی است";
        isOk = false;
      }

      if (isOk) {
        this.todos.push({
          id: this.todosLenght+1,
          title: t,
          subject: s,
          body: b,
          important: this.important,
          color: this.color,
          status: false
        });
        Swal.fire({
          title: 'موفق',
          text: 'کار با موفقیت اضافه شد',
          icon: 'success',
          confirmButtonText: 'بستن'
        });

        this.clearError();

        this.title = "";
        this.subject = "";
        this.body = "";
      }

    },
    clearError(){
      this.eTitle = "";
      this.eSubject = "";
      this.eBody = "";
    },
    changeStatus(todo) {
      if (todo.status) {
        todo.status = false;
      } else {
        todo.status = true;
      }
    },
    removeTodo(id) {
      console.log(id);
      this.todos.splice(this.todos.findIndex(todo=>todo.id == id), 1);
      Swal.fire({
        title: 'موفق',
        text: 'کار با موفقیت پاک شد',
        icon: 'success',
        confirmButtonText: 'بستن'
      });
    },
  },
  components: {
    Header, Footer, Card
  },
  data() {
    return {
      visibility: 'all',
      colors: ['آبی', 'مشکی', 'قرمز'],
      importants: ['کم', 'متوسط', 'زیاد'],
      eTitle: "", eBody: "", eSubject: "",
      title: "",
      subject: "",
      body: "",
      color: "مشکی",
      important: 'کم',
      status: false,
      todos: [

      ],
    }
  }
}
</script>

<style>
* {
  font-family: "mFont";
}

a {
  text-decoration: none;
}

@font-face {
  font-family: "mFont";
  src: url('./fonts/font.TTF') ;
}
</style>
