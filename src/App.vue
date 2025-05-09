<template>
  <div class="app-container" dir="ltr">
    <Header class="animate__animated animate__fadeIn"></Header>

    <div class="container py-5">
      <div class="row g-5">
        <!-- New Task Form -->
        <div class="col-lg-6 order-lg-2">
          <div class="task-container animate__animated animate__fadeInRight">
            <div class="task-header">
              <div class="d-flex align-items-center">
                <div class="header-icon">
                  <i class="bi bi-plus-circle-fill"></i>
                </div>
                <div class="header-text">
                  <h4 class="mb-0">New Task</h4>
                  <p class="mb-0 text-white-50">Create a new task to organize your work</p>
                </div>
              </div>
            </div>
            <div class="task-content">
              <form @submit.prevent="add()" class="needs-validation" novalidate>
                <div class="form-group">
                  <div class="input-wrapper">
                    <label class="form-label">Title</label>
                    <div class="input-group">
                      <span class="input-group-text"><i class="bi bi-pencil-square"></i></span>
                      <input v-model="title" type="text" class="form-control" :class="{'is-invalid': eTitle}" placeholder="Enter task title">
                    </div>
                    <div class="invalid-feedback">{{ eTitle }}</div>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-wrapper">
                    <label class="form-label">Subject</label>
                    <div class="input-group">
                      <span class="input-group-text"><i class="bi bi-tag"></i></span>
                      <input v-model="subject" type="text" class="form-control" :class="{'is-invalid': eSubject}" placeholder="Enter task subject">
                    </div>
                    <div class="invalid-feedback">{{ eSubject }}</div>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-wrapper">
                    <label class="form-label">Description</label>
                    <div class="input-group">
                      <span class="input-group-text"><i class="bi bi-text-paragraph"></i></span>
                      <textarea v-model="body" class="form-control" rows="3" :class="{'is-invalid': eBody}" placeholder="Enter task description"></textarea>
                    </div>
                    <div class="invalid-feedback">{{ eBody }}</div>
                  </div>
                </div>

                <div class="form-row">
                  <div class="form-group col-md-6">
                    <div class="input-wrapper">
                      <label class="form-label">Importance</label>
                      <div class="input-group">
                        <span class="input-group-text"><i class="bi bi-flag"></i></span>
                        <select v-model="important" class="form-select">
                          <option v-for="(important, index) in importants" :key="index">{{ important }}</option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <div class="form-group col-md-6">
                    <div class="input-wrapper">
                      <label class="form-label">Color</label>
                      <div class="input-group">
                        <span class="input-group-text"><i class="bi bi-palette"></i></span>
                        <select v-model="color" class="form-select">
                          <option v-for="(color, index) in colors" :key="index">{{ color }}</option>
                        </select>
                      </div>
                    </div>
                  </div>
                </div>

                <button class="btn w-100 text-white mt-4 submit-btn" type="submit">
                  <i class="bi bi-plus-circle me-2"></i>Save Task
                </button>
              </form>
            </div>
          </div>
        </div>

        <!-- Todo List -->
        <div class="col-lg-6 order-lg-1">
          <div class="task-container animate__animated animate__fadeInLeft">
            <div class="task-header">
              <div class="task-header-content">
                <div class="task-header-icon">
                  <i class="bi bi-list-check"></i>
                </div>
                <div class="task-header-text">
                  <h4>Task List</h4>
                  <span class="task-count">
                    <i class="bi bi-check2-all"></i>
                    {{ todosLenght }} Tasks
                  </span>
                </div>
              </div>
              <div class="task-filters">
                <button @click="visibility='all'" 
                        class="filter-btn" 
                        :class="{'active': visibility === 'all'}">
                  <i class="bi bi-grid"></i>
                  <span>All Tasks</span>
                  <span class="count">{{ todos.length }}</span>
                </button>
                <button @click="visibility='active'" 
                        class="filter-btn" 
                        :class="{'active': visibility === 'active'}">
                  <i class="bi bi-check-circle"></i>
                  <span>Active</span>
                  <span class="count">{{ activeTodos.length }}</span>
                </button>
                <button @click="visibility='completed'" 
                        class="filter-btn" 
                        :class="{'active': visibility === 'completed'}">
                  <i class="bi bi-check2-square"></i>
                  <span>Completed</span>
                  <span class="count">{{ completedTodos.length }}</span>
                </button>
              </div>
            </div>

            <div class="task-content">
              <div v-if="todos.length === 0" class="empty-state">
                <div class="empty-state-icon">
                  <i class="bi bi-clipboard-check"></i>
                </div>
                <h3>No Tasks Yet</h3>
                <p>Start organizing your tasks by adding your first one.</p>
                <button class="add-task-btn" @click="showAddModal = true">
                  <i class="bi bi-plus-lg"></i>
                  Add New Task
                </button>
              </div>

              <div v-else class="task-list">
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
    },
    activeTodos() {
      return this.todos.filter(todo => !todo.status);
    },
    completedTodos() {
      return this.todos.filter(todo => todo.status);
    }
  },
  methods: {
    add() {
      this.clearError();
      let isValid = true;

      if (!this.title.trim()) {
        this.eTitle = "This field is required";
        isValid = false;
      }
      if (!this.subject.trim()) {
        this.eSubject = "This field is required";
        isValid = false;
      }
      if (!this.body.trim()) {
        this.eBody = "This field is required";
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
          title: 'Success',
          text: 'Task added successfully',
          icon: 'success',
          confirmButtonText: 'Close',
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
      this.color = "Black";
      this.important = "Low";
    },
    changeStatus(todo) {
      todo.status = !todo.status;
    },
    removeTodo(id) {
      Swal.fire({
        title: 'Are you sure?',
        text: "This action cannot be undone!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#3085d6',
        confirmButtonText: 'Yes, delete it!',
        cancelButtonText: 'Cancel'
      }).then((result) => {
        if (result.isConfirmed) {
          this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1);
          Swal.fire({
            title: 'Deleted!',
            text: 'Task has been deleted.',
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
      colors: ['Blue', 'Black', 'Red'],
      importants: ['Low', 'Medium', 'High'],
      eTitle: "",
      eBody: "",
      eSubject: "",
      title: "",
      subject: "",
      body: "",
      color: "Black",
      important: 'Low',
      todos: [],
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Naskh+Arabic:wght@400;500;600;700&display=swap');
@import 'bootstrap/dist/css/bootstrap.min.css';
@import 'bootstrap-icons/font/bootstrap-icons.css';
@import 'animate.css';

:root {
  --primary-color: #42b883;
  --secondary-color: #35495e;
  --success-color: #42b883;
  --danger-color: #ff4757;
  --warning-color: #ffa502;
  --info-color: #2ed573;
  --dark-color: #2c3e50;
  --light-color: #f8f9fa;
  --vue-gradient: linear-gradient(135deg, #42b883 0%, #35495e 100%);
  --primary-color-rgb: 66, 184, 131;
  --secondary-color-rgb: 53, 73, 94;
  --card-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --hover-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  --border-radius: 16px;
  --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  --border-color: rgba(255, 255, 255, 0.2);
}

body {
  font-family: 'Vazirmatn', sans-serif;
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  color: var(--dark-color);
  line-height: 1.6;
  text-align: left;
  min-height: 100vh;
}

.header-icon {
  width: 2.5rem;
  height: 2.5rem;
  background: rgba(255, 255, 255, 0.15);
  border-radius: var(--border-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 1rem;
  transition: var(--transition);
  backdrop-filter: blur(5px);
  box-shadow: none;

  i {
    font-size: 1.25rem;
    color: white;
  }

  &:hover {
    transform: scale(1.05);
    background: rgba(255, 255, 255, 0.2);
  }
}

.input-group {
  .input-group-text {
    background-color: rgba(255, 255, 255, 0.9);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-right: none;
    color: var(--primary-color);
    font-size: 1.1rem;
    padding: 0.75rem 1rem;
    transition: var(--transition);
    border-radius: var(--border-radius) 0 0 var(--border-radius);
    backdrop-filter: blur(5px);
  }

  .form-control, .form-select {
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-left: none;
    padding: 0.75rem 1rem;
    font-size: 1rem;
    transition: var(--transition);
    border-radius: 0 var(--border-radius) var(--border-radius) 0;
    background-color: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(5px);
    
    &:focus {
      border-color: var(--primary-color);
      box-shadow: 0 0 0 4px rgba(var(--primary-color-rgb), 0.1);
      background-color: #ffffff;
    }
  }

  &:focus-within {
    .input-group-text {
      border-color: var(--primary-color);
      background-color: var(--primary-color);
      color: white;
    }
  }
}

.form-label {
  font-weight: 600;
  color: var(--dark-color);
  margin-bottom: 0.75rem;
  font-size: 0.95rem;
  transition: var(--transition);
  opacity: 0.9;
  
  &:hover {
    opacity: 1;
  }
}

.submit-btn {
  background: var(--vue-gradient);
  border: none;
  color: white;
  padding: 0.6rem 1.25rem;
  font-size: 0.9rem;
  font-weight: 500;
  border-radius: 10px;
  transition: all 0.2s ease;
  box-shadow: 0 2px 8px rgba(var(--primary-color-rgb), 0.15);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  
  i {
    font-size: 0.85rem;
    color: white;
    transition: transform 0.2s ease;
    opacity: 0.9;
  }
  
  &:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(var(--primary-color-rgb), 0.2);
    background: linear-gradient(135deg, #3aa876 0%, #2c3e50 100%);
    
    i {
      transform: scale(1.1);
      opacity: 1;
    }
  }
  
  &:active {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(var(--primary-color-rgb), 0.1);
  }
}

.card {
  border: none;
  border-radius: var(--border-radius);
  box-shadow: none;
  transition: var(--transition);
  background-color: rgba(255, 255, 255, 0.95);
  overflow: hidden;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  position: relative;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, rgba(var(--primary-color-rgb), 0.05) 0%, rgba(var(--secondary-color-rgb), 0.05) 100%);
    z-index: -1;
  }
  
  .card-header {
    background: var(--vue-gradient);
    border-bottom: none;
    padding: 2.5rem;
    position: relative;
    overflow: hidden;
    
    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.1'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
      opacity: 0.5;
    }
    
    h4 {
      font-weight: 700;
      font-size: 1.35rem;
      margin: 0;
      color: white;
      display: flex;
      align-items: center;
      gap: 0.75rem;
      position: relative;
    }
  }
  
  .card-body {
    padding: 2.5rem;
  }
  
  &:hover {
    transform: translateY(-4px);
    box-shadow: var(--hover-shadow);
    
    .header-icon {
      transform: scale(1.1);
      background: rgba(255, 255, 255, 0.3);
    }
  }
}

.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%2342b883' fill-opacity='0.05'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    opacity: 0.5;
    z-index: 0;
  }
}

.task-container {
  background: rgba(255, 255, 255, 0.95);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  overflow: hidden;
  position: relative;
  z-index: 1;
  transition: var(--transition);

  .task-header {
    padding: 2rem;
    background: var(--vue-gradient);
    position: relative;
    overflow: hidden;

    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.1'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
      opacity: 0.5;
    }

    .header-icon {
      width: 3rem;
      height: 3rem;
      background: rgba(255, 255, 255, 0.15);
      border-radius: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-right: 1rem;
      transition: var(--transition);
      backdrop-filter: blur(5px);

      i {
        font-size: 1.25rem;
        color: white;
      }

      &:hover {
        transform: scale(1.05);
        background: rgba(255, 255, 255, 0.2);
      }
    }

    .header-text {
      h4 {
        font-size: 1.5rem;
        font-weight: 700;
        color: white;
        margin-bottom: 0.25rem;
      }

      p {
        font-size: 0.95rem;
        opacity: 0.8;
      }
    }
  }

  .task-content {
    padding: 2rem;
    background: rgba(255, 255, 255, 0.8);
    backdrop-filter: blur(10px);

    .form-group {
      margin-bottom: 1.5rem;

      .input-wrapper {
        .form-label {
          font-size: 0.9rem;
          font-weight: 600;
          color: var(--dark-color);
          margin-bottom: 0.5rem;
          opacity: 0.9;
        }

        .input-group {
          .input-group-text {
            background: rgba(255, 255, 255, 0.9);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-right: none;
            color: var(--primary-color);
            font-size: 1rem;
            padding: 0.75rem 1rem;
            transition: var(--transition);
            border-radius: 12px 0 0 12px;
          }

          .form-control, .form-select {
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-left: none;
            padding: 0.75rem 1rem;
            font-size: 0.95rem;
            transition: var(--transition);
            border-radius: 0 12px 12px 0;
            background: rgba(255, 255, 255, 0.9);
            
            &:focus {
              border-color: var(--primary-color);
              box-shadow: 0 0 0 4px rgba(var(--primary-color-rgb), 0.1);
              background: #ffffff;
            }

            &::placeholder {
              color: #a0aec0;
              opacity: 0.7;
            }
          }

          &:focus-within {
            .input-group-text {
              border-color: var(--primary-color);
              background: var(--primary-color);
              color: white;
            }
          }
        }

        .invalid-feedback {
          font-size: 0.85rem;
          margin-top: 0.5rem;
        }
      }
    }

    .form-row {
      display: flex;
      flex-wrap: wrap;
      margin-right: -0.75rem;
      margin-left: -0.75rem;

      .form-group {
        padding-right: 0.75rem;
        padding-left: 0.75rem;
        flex: 0 0 50%;
        max-width: 50%;
      }
    }
  }
}

.task-header {
  padding: 2.5rem;
  background: var(--vue-gradient);
  color: white;
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.1'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    opacity: 0.5;
  }
}

.task-header-content {
  display: flex;
  align-items: center;
  gap: 1.75rem;
  margin-bottom: 2.5rem;
  position: relative;
}

.task-header-icon {
  width: 4rem;
  height: 4rem;
  background: rgba(255, 255, 255, 0.15);
  border-radius: var(--border-radius);
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(5px);
  box-shadow: var(--card-shadow);
  transition: var(--transition);

  &:hover {
    transform: scale(1.05);
    background: rgba(255, 255, 255, 0.2);
  }

  i {
    font-size: 2rem;
    color: white;
  }
}

.task-header-text {
  h4 {
    font-size: 2rem;
    font-weight: 800;
    margin: 0 0 0.75rem;
    letter-spacing: -0.02em;
  }
}

.task-count {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 1.1rem;
  opacity: 0.9;
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 1rem;
  border-radius: var(--border-radius);
  backdrop-filter: blur(5px);

  i {
    font-size: 1.2rem;
  }
}

.task-filters {
  display: flex;
  gap: 0.5rem;
  flex-wrap: nowrap;
  position: relative;
  justify-content: flex-start;
  margin-top: 1rem;
}

.filter-btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 0.9rem;
  font-weight: 500;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
  transition: var(--transition);
  min-width: auto;
  justify-content: center;
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(5px);
  white-space: nowrap;

  i {
    font-size: 1rem;
    transition: var(--transition);
  }

  .count {
    background: rgba(255, 255, 255, 0.2);
    padding: 0.25rem 0.5rem;
    border-radius: 6px;
    font-size: 0.8rem;
    margin-left: 0.25rem;
    transition: var(--transition);
    backdrop-filter: blur(5px);
  }

  &:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: translateY(-1px);
    box-shadow: var(--card-shadow);

    i {
      transform: scale(1.1);
    }
  }

  &.active {
    background: white;
    color: var(--primary-color);
    box-shadow: var(--card-shadow);

    .count {
      background: rgba(var(--primary-color-rgb), 0.1);
    }
  }
}

.task-content {
  padding: 2.5rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
}

.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  background: rgba(255, 255, 255, 0.8);
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  animation: fadeIn 0.5s ease;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  margin: 1rem 0;
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, rgba(var(--primary-color-rgb), 0.05) 0%, rgba(var(--secondary-color-rgb), 0.05) 100%);
    z-index: -1;
  }

  .empty-state-icon {
    width: 6rem;
    height: 6rem;
    background: var(--vue-gradient);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto 2rem;
    animation: float 3s ease-in-out infinite;
    box-shadow: var(--card-shadow);

    i {
      font-size: 3rem;
      color: white;
    }
  }

  h3 {
    font-size: 2rem;
    font-weight: 800;
    color: var(--dark-color);
    margin-bottom: 1rem;
    letter-spacing: -0.02em;
  }

  p {
    color: #6c757d;
    margin: 0 0 2rem;
    font-size: 1.1rem;
    line-height: 1.6;
    max-width: 400px;
    margin-left: auto;
    margin-right: auto;
  }
}

.add-task-btn {
  padding: 1.25rem 2.5rem;
  background: var(--vue-gradient);
  border: none;
  border-radius: var(--border-radius);
  color: white;
  font-size: 1.1rem;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  transition: var(--transition);
  box-shadow: var(--card-shadow);
  position: relative;
  overflow: hidden;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(255, 255, 255, 0.1);
    transform: translateX(-100%);
    transition: transform 0.3s ease;
  }

  i {
    font-size: 1.3rem;
    transition: var(--transition);
  }

  &:hover {
    transform: translateY(-2px);
    box-shadow: var(--hover-shadow);

    &::before {
      transform: translateX(0);
    }

    i {
      transform: scale(1.2);
    }
  }
}

.task-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 2.5rem;
  margin: 1rem 0;
}

@media (max-width: 768px) {
  .task-header {
    padding: 2rem;
  }

  .task-header-content {
    gap: 1.25rem;
    margin-bottom: 2rem;
  }

  .task-header-icon {
    width: 3.5rem;
    height: 3.5rem;

    i {
      font-size: 1.75rem;
    }
  }

  .task-header-text {
    h4 {
      font-size: 1.75rem;
    }
  }

  .task-filters {
    gap: 0.35rem;
    overflow-x: auto;
    padding-bottom: 0.5rem;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
    &::-webkit-scrollbar {
      display: none;
    }
  }

  .filter-btn {
    padding: 0.4rem 0.75rem;
    font-size: 0.85rem;
    
    i {
      font-size: 0.9rem;
    }
    
    .count {
      padding: 0.2rem 0.4rem;
      font-size: 0.75rem;
    }
  }

  .task-content {
    padding: 2rem;
  }

  .task-list {
    grid-template-columns: 1fr;
    gap: 2rem;
  }

  .empty-state {
    padding: 3rem 1.5rem;
    margin: 0.5rem 0;

    .empty-state-icon {
      width: 5rem;
      height: 5rem;
      margin-bottom: 1.5rem;

      i {
        font-size: 2.5rem;
      }
    }

    h3 {
      font-size: 1.75rem;
    }

    p {
      font-size: 1rem;
      margin-bottom: 1.5rem;
    }

    .add-task-btn {
      padding: 1rem 2rem;
      font-size: 1rem;
    }
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
