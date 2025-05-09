<template>
  <div class="todo-card" :class="{'completed': todo.status}">
    <div class="card-pattern"></div>
    <div class="todo-header">
      <div class="todo-title">
        <div class="title-wrapper">
          <div class="title-content">
            <h3>{{ todo.title }}</h3>
            <div class="todo-badges">
              <span class="badge" :class="getImportanceClass(todo.important)">
                <i class="bi bi-flag-fill me-1"></i>
                {{ todo.important }}
              </span>
              <span class="badge" :class="getColorClass(todo.color)">
                <i class="bi bi-palette-fill me-1"></i>
                {{ todo.color }}
              </span>
            </div>
          </div>
          <div class="todo-actions">
            <button @click="$emit('completed', todo)" class="btn btn-sm action-btn" :class="todo.status ? 'btn-success' : 'btn-primary'">
              <i :class="todo.status ? 'bi-check2-circle' : 'bi-check-circle'"></i>
              <span>{{ todo.status ? 'Undo' : 'Complete' }}</span>
            </button>
            <button @click="$emit('remove', todo.id)" class="btn btn-sm action-btn btn-danger">
              <i class="bi bi-trash3"></i>
              <span>Delete</span>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="todo-body">
      <div class="todo-subject">
        <div class="subject-icon">
          <i class="bi bi-tag-fill"></i>
        </div>
        <span>{{ todo.subject }}</span>
      </div>
      <div class="todo-text-wrapper">
        <div class="text-icon">
          <i class="bi bi-text-paragraph"></i>
        </div>
        <p class="todo-text">{{ todo.body }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoCard',
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    getImportanceClass(important) {
      switch(important) {
        case 'Low':
          return 'bg-success';
        case 'Medium':
          return 'bg-warning';
        case 'High':
          return 'bg-danger';
        default:
          return 'bg-primary';
      }
    },
    getColorClass(color) {
      switch(color) {
        case 'Blue':
          return 'bg-primary';
        case 'Black':
          return 'bg-dark';
        case 'Red':
          return 'bg-danger';
        default:
          return 'bg-secondary';
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.todo-card {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 1.5rem;
  margin-bottom: 0;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);

  .card-pattern {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%2342b883' fill-opacity='0.03'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    opacity: 0;
    transition: opacity 0.4s ease;
  }

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--vue-gradient);
    opacity: 0;
    transition: opacity 0.3s ease;
    z-index: 0;
  }

  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 4px;
    height: 100%;
    background: var(--vue-gradient);
    opacity: 0;
    transition: opacity 0.4s ease;
  }

  &:hover {
    transform: translateY(-4px);
    box-shadow: 0 12px 20px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -5px rgba(0, 0, 0, 0.04);

    .card-pattern {
      opacity: 1;
    }

    &::before {
      opacity: 0.05;
    }

    .todo-actions {
      opacity: 1;
      transform: translateX(0);
    }

    .todo-subject {
      transform: translateX(4px);
      background: rgba(var(--primary-color-rgb), 0.12);
    }

    .subject-icon {
      transform: scale(1.1);
      background: var(--primary-color);
      color: white;
    }

    .text-icon {
      transform: scale(1.1);
      background: var(--secondary-color);
      color: white;
    }

    .todo-badges .badge {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
  }

  &.completed {
    opacity: 0.8;
    
    .todo-title h3 {
      text-decoration: line-through;
      color: #6c757d;
    }

    .todo-subject {
      opacity: 0.7;
    }

    .todo-text {
      opacity: 0.7;
    }

    .subject-icon, .text-icon {
      opacity: 0.7;
    }
  }
}

.todo-header {
  margin-bottom: 1.25rem;
}

.todo-title {
  position: relative;
  z-index: 1;

  .title-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 1rem;
  }

  .title-content {
    flex: 1;
    padding-right: 1rem;
  }

  h3 {
    font-size: 1.2rem;
    font-weight: 600;
    color: var(--dark-color);
    margin: 0 0 0.75rem;
    line-height: 1.4;
    transition: color 0.3s ease;
  }
}

.todo-badges {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  position: relative;
  z-index: 1;

  .badge {
    padding: 0.4em 0.8em;
    font-size: 0.8rem;
    font-weight: 500;
    border-radius: 10px;
    display: inline-flex;
    align-items: center;
    transition: all 0.3s ease;
    color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: relative;
    overflow: hidden;

    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0));
      opacity: 0;
      transition: opacity 0.3s ease;
    }

    i {
      font-size: 0.75rem;
      margin-right: 0.3rem;
      position: relative;
      z-index: 1;
      opacity: 0.9;
    }

    &.bg-success {
      background: linear-gradient(135deg, #2ecc71, #27ae60);
    }

    &.bg-warning {
      background: linear-gradient(135deg, #f1c40f, #f39c12);
    }

    &.bg-danger {
      background: linear-gradient(135deg, #e74c3c, #c0392b);
    }

    &.bg-primary {
      background: linear-gradient(135deg, #3498db, #2980b9);
    }

    &.bg-dark {
      background: linear-gradient(135deg, #34495e, #2c3e50);
    }

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);

      &::before {
        opacity: 1;
      }

      i {
        opacity: 1;
      }
    }
  }
}

.todo-actions {
  display: flex;
  gap: 0.5rem;
  opacity: 0;
  transform: translateX(10px);
  transition: all 0.3s ease;

  .action-btn {
    padding: 0.6rem 1rem;
    font-size: 0.9rem;
    border-radius: 10px;
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    transition: all 0.3s ease;
    color: white;
    border: none;
    font-weight: 500;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    min-width: 100px;
    justify-content: center;

    i {
      font-size: 1.1rem;
    }

    &.btn-primary {
      background: var(--vue-gradient);
    }

    &.btn-success {
      background: linear-gradient(135deg, #2ecc71, #27ae60);
    }

    &.btn-danger {
      background: linear-gradient(135deg, #e74c3c, #c0392b);
    }

    &:hover {
      transform: translateY(-1px);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    }
  }
}

.todo-body {
  position: relative;
  z-index: 1;

  .todo-subject {
    font-size: 1rem;
    color: var(--primary-color);
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-weight: 500;
    padding: 0.75rem;
    background: rgba(var(--primary-color-rgb), 0.08);
    border-radius: 12px;
    transition: all 0.3s ease;

    .subject-icon {
      width: 2rem;
      height: 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(var(--primary-color-rgb), 0.1);
      border-radius: 10px;
      transition: all 0.3s ease;

      i {
        font-size: 1.1rem;
        color: var(--primary-color);
      }
    }

    &:hover {
      background: rgba(var(--primary-color-rgb), 0.12);
      transform: translateX(2px);
    }
  }

  .todo-text-wrapper {
    padding: 0.75rem;
    background: rgba(var(--secondary-color-rgb), 0.03);
    border-radius: 12px;
    display: flex;
    gap: 0.75rem;
    transition: all 0.3s ease;
    
    .text-icon {
      width: 2rem;
      height: 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(var(--secondary-color-rgb), 0.1);
      border-radius: 10px;
      transition: all 0.3s ease;

      i {
        font-size: 1.1rem;
        color: var(--secondary-color);
      }
    }

    &:hover {
      background: rgba(var(--secondary-color-rgb), 0.05);
      transform: translateX(2px);
    }
  }

  .todo-text {
    font-size: 1rem;
    color: #4a5568;
    line-height: 1.6;
    margin: 0;
    flex: 1;
  }
}

@media (max-width: 576px) {
  .todo-card {
    padding: 1.25rem;
  }

  .todo-title {
    .title-wrapper {
      flex-direction: column;
      gap: 0.75rem;
    }

    .title-content {
      padding-right: 0;
    }

    h3 {
      font-size: 1.15rem;
    }
  }

  .todo-actions {
    opacity: 1;
    transform: none;
    width: 100%;
    justify-content: flex-end;
  }

  .todo-badges {
    .badge {
      padding: 0.4em 0.8em;
      font-size: 0.8rem;
    }
  }

  .todo-subject {
    padding: 0.6rem;

    .subject-icon {
      width: 1.75rem;
      height: 1.75rem;

      i {
        font-size: 1rem;
      }
    }
  }

  .todo-text-wrapper {
    padding: 0.6rem;

    .text-icon {
      width: 1.75rem;
      height: 1.75rem;

      i {
        font-size: 1rem;
      }
    }
  }

  .todo-text {
    font-size: 0.95rem;
  }

  .action-btn {
    min-width: 90px;
    padding: 0.5rem 0.75rem;
    font-size: 0.85rem;
  }
}
</style>