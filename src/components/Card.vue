<template>
  <div class="todo-card" :class="{'completed': todo.status}">
    <div class="todo-header">
      <div class="todo-title">
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
        <button @click="$emit('completed', todo)" class="btn btn-sm" :class="todo.status ? 'btn-success' : 'btn-primary'">
          <i :class="todo.status ? 'bi-check2-circle' : 'bi-check-circle'"></i>
          {{ todo.status ? 'Undo' : 'Complete' }}
        </button>
        <button @click="$emit('remove', todo.id)" class="btn btn-sm btn-danger">
          <i class="bi bi-trash3"></i>
          Delete
        </button>
      </div>
    </div>
    <div class="todo-body">
      <div class="todo-subject">
        <i class="bi bi-tag-fill"></i>
        {{ todo.subject }}
      </div>
      <p class="todo-text">{{ todo.body }}</p>
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
  background: rgba(255, 255, 255, 0.9);
  border-radius: 24px;
  padding: 2.5rem;
  margin-bottom: 0;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 4px;
    height: 100%;
    background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
    opacity: 0;
    transition: opacity 0.4s ease;
  }

  &:hover {
    transform: translateY(-6px) scale(1.02);
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
    border-color: var(--primary-color);

    &::before {
      opacity: 1;
    }

    .todo-actions {
      opacity: 1;
      transform: translateX(0);
    }
  }

  &.completed {
    opacity: 0.7;
    
    .todo-title h3 {
      text-decoration: line-through;
      color: #6c757d;
    }
  }
}

.todo-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 2rem;
}

.todo-title {
  h3 {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--dark-color);
    margin-bottom: 1rem;
    transition: all 0.3s ease;
    line-height: 1.4;
    letter-spacing: -0.01em;
  }
}

.todo-badges {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;

  .badge {
    padding: 0.7em 1.2em;
    font-size: 0.9rem;
    font-weight: 600;
    border-radius: 12px;
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
      font-size: 1rem;
      color: white;
      margin-right: 0.6rem;
      position: relative;
      z-index: 1;
    }

    span {
      position: relative;
      z-index: 1;
    }

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);

      &::before {
        opacity: 1;
      }
    }
  }
}

.todo-actions {
  display: flex;
  gap: 1rem;
  opacity: 0;
  transform: translateX(10px);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);

  .btn {
    padding: 0.8rem 1.2rem;
    font-size: 0.95rem;
    border-radius: 12px;
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    transition: all 0.3s ease;
    color: white;
    border: none;
    font-weight: 600;
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
      font-size: 1.2rem;
      transition: all 0.3s ease;
      color: white;
      position: relative;
      z-index: 1;
    }

    span {
      position: relative;
      z-index: 1;
    }

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);

      &::before {
        opacity: 1;
      }

      i {
        transform: scale(1.2);
      }
    }

    &.btn-primary {
      background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
    }

    &.btn-success {
      background: linear-gradient(135deg, #2ecc71, #27ae60);
    }

    &.btn-danger {
      background: linear-gradient(135deg, #e74c3c, #c0392b);
    }
  }
}

.todo-body {
  .todo-subject {
    font-size: 1.1rem;
    color: var(--primary-color);
    margin-bottom: 1.25rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-weight: 600;
    padding: 1rem;
    background: rgba(var(--primary-color-rgb), 0.08);
    border-radius: 12px;
    transition: all 0.3s ease;

    &:hover {
      background: rgba(var(--primary-color-rgb), 0.12);
      transform: translateX(4px);
    }

    i {
      font-size: 1.2rem;
      color: var(--primary-color);
    }
  }

  .todo-text {
    font-size: 1.1rem;
    color: #4a5568;
    line-height: 1.7;
    margin: 0;
    padding: 0 0.5rem;
  }
}

@media (max-width: 576px) {
  .todo-card {
    padding: 1.75rem;
  }

  .todo-header {
    flex-direction: column;
    gap: 1.25rem;
  }

  .todo-actions {
    opacity: 1;
    transform: none;
    width: 100%;
    justify-content: flex-end;
  }

  .todo-title h3 {
    font-size: 1.35rem;
  }

  .todo-badges {
    gap: 0.75rem;
  }

  .badge {
    padding: 0.6em 1em;
    font-size: 0.85rem;
  }

  .todo-subject {
    font-size: 1rem;
    padding: 0.85rem;
  }

  .todo-text {
    font-size: 1rem;
  }
}
</style>