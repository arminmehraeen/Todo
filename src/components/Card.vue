<template>
  <li class="list-group-item d-flex justify-content-between mb-2">
    <div class="p-3">
      <h6 class="my-0">{{ todo.title }}</h6>
      <br>
      <h6 v-if="!todo.status" class="text-success">وضعیت : فعال</h6>
      <h6 v-else class="text-danger">وضعیت : تمام شده</h6>
      <h6 class="text-muted">موضوع : {{ todo.subject }}</h6>
      <h6 class="text-muted">رنگ : {{ todo.color }}</h6>
      <h6 class="text-muted">اهمیت : {{ todo.important }}</h6>
      <h6 class="text-muted">توضیحات : {{ todo.body }}</h6>
      <hr>
      <button v-if="todo.status" @click="completed(todo)" class="btn btn-sm btn-outline-primary ms-1">برگشت به لیست فعال</button>
      <button v-else @click="completed(todo)" class="btn btn-sm btn-outline-success ms-1">انجام کار</button>
      <button @click="remove(todo.id)" class="btn btn-sm btn-outline-danger">حذف</button>
    </div>
  </li>
</template>

<script>
export default {
  name: "Card",
  props: {todo: Object},
  methods:{
    completed(todo){
      this.$emit("completed",todo);
    },
    remove(id){
      this.$emit("remove",id);
    }
  },
}
</script>

<style scoped></style>