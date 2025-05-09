<template>
  <div class="todo-card mb-3" :class="{'completed': todo.status}">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-start">
        <div class="todo-content">
          <h5 class="card-title mb-2">{{ todo.title }}</h5>
          <p class="card-subtitle mb-2 text-muted">{{ todo.subject }}</p>
          <p class="card-text">{{ todo.body }}</p>
          
          <div class="todo-meta">
            <span class="badge" :class="getStatusClass">
              {{ todo.status ? 'تمام شده' : 'فعال' }}
            </span>
            <span class="badge" :class="getImportanceClass">
              {{ todo.important }}
            </span>
            <span class="badge" :class="getColorClass">
              {{ todo.color }}
            </span>
          </div>
        </div>
        
        <div class="todo-actions">
          <button @click="completed(todo)" 
                  class="btn btn-sm" 
                  :class="todo.status ? 'btn-outline-primary' : 'btn-outline-success'">
            <i :class="todo.status ? 'bi bi-arrow-counterclockwise' : 'bi bi-check-lg'"></i>
            {{ todo.status ? 'برگشت' : 'انجام' }}
          </button>
          <button @click="remove(todo.id)" 
                  class="btn btn-sm btn-outline-danger ms-2">
            <i class="bi bi-trash"></i>
            حذف
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  computed: {
    getStatusClass() {
      return {
        'bg-success': !this.todo.status,
        'bg-danger': this.todo.status
      }
    },
    getImportanceClass() {
      const classes = {
        'کم': 'bg-info',
        'متوسط': 'bg-warning',
        'زیاد': 'bg-danger'
      }
      return classes[this.todo.important]
    },
    getColorClass() {
      const classes = {
        'آبی': 'bg-primary',
        'مشکی': 'bg-dark',
        'قرمز': 'bg-danger'
      }
      return classes[this.todo.color]
    }
  },
  methods: {
    completed(todo) {
      this.$emit("completed", todo);
    },
    remove(id) {
      this.$emit("remove", id);
    }
  }
}
</script>

<style lang="scss" scoped>
.todo-card {
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }

  &.completed {
    opacity: 0.8;
    .card-title {
      text-decoration: line-through;
    }
  }

  .card-body {
    padding: 1.25rem;
  }

  .todo-content {
    flex: 1;
  }

  .todo-meta {
    margin-top: 1rem;
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .todo-actions {
    display: flex;
    gap: 0.5rem;
  }

  .badge {
    padding: 0.5em 0.8em;
    font-weight: 500;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    border-radius: 20px;
    padding: 0.4rem 1rem;
    
    i {
      font-size: 1rem;
    }
  }
}
</style>