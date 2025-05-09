<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <div class="header-left">
          <div class="header-logo">
            <img src="@/assets/logo.png" alt="Vue.js Logo" class="logo">
            <span class="brand">Todo App</span>
          </div>
          <p class="header-subtitle">Simple & Modern Task Management</p>
        </div>
        <div class="header-features">
          <div class="feature-item">
            <i class="bi bi-magic"></i>
            <span>Simple</span>
          </div>
          <div class="feature-item">
            <i class="bi bi-lightning-charge-fill"></i>
            <span>Fast</span>
          </div>
          <div class="feature-item">
            <i class="bi bi-shield-check"></i>
            <span>Secure</span>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader'
}
</script>

<style lang="scss" scoped>
.header {
  background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  position: relative;
  overflow: hidden;
  border-radius: 0 0 30px 30px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23ffffff' fill-opacity='0.1' fill-rule='evenodd'/%3E%3C/svg%3E");
    opacity: 0.1;
  }
}

.header-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  animation: fadeInUp 0.8s ease;
  padding: 1.5rem 0;
}

.header-left {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  animation: fadeInUp 0.8s ease 0.2s backwards;
  text-align: center;
}

.header-logo {
  display: flex;
  align-items: center;
  gap: 1rem;

  .logo {
    width: 40px;
    height: auto;
    filter: brightness(0) invert(1);
    transition: all 0.3s ease;
    animation: float 3s ease-in-out infinite;

    &:hover {
      transform: scale(1.1);
    }
  }

  .brand {
    font-size: 1.5rem;
    font-weight: 700;
    color: white;
  }
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1rem;
  margin: 0;
}

.header-features {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
  animation: fadeInUp 0.8s ease 0.4s backwards;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 1rem;
  border-radius: 50px;
  color: white;
  font-size: 0.9rem;
  font-weight: 500;
  transition: all 0.3s ease;
  cursor: pointer;
  min-width: 100px;
  justify-content: center;
  
  i {
    font-size: 1rem;
    color: white;
    transition: all 0.3s ease;
  }
  
  &:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: translateY(-2px);
    
    i {
      transform: scale(1.2);
    }
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0px);
  }
}

@media (max-width: 768px) {
  .header-content {
    padding: 1.25rem 0;
  }

  .header-features {
    gap: 0.75rem;
  }

  .feature-item {
    padding: 0.4rem 0.8rem;
    font-size: 0.85rem;
    min-width: 90px;
  }
}
</style>