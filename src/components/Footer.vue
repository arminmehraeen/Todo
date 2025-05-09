<template>
  <footer class="footer mt-auto py-4">
    <div class="container">
      <div class="row align-items-center">
        <div class="col-md-6 text-center text-md-end">
          <p class="mb-0">&copy; {{ currentYear }} | VueJs Todo App</p>
        </div>
        <div class="col-md-6 text-center text-md-start">
          <ul class="list-inline mb-0">
            <li class="list-inline-item">
              <a href="#" class="footer-link">
                <i class="bi bi-shield-check"></i>
                امنیت
              </a>
            </li>
            <li class="list-inline-item">
              <a href="#" class="footer-link">
                <i class="bi bi-file-text"></i>
                قوانین
              </a>
            </li>
            <li class="list-inline-item">
              <a href="#" class="footer-link">
                <i class="bi bi-headset"></i>
                پشتیبانی
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: "Footer",
  computed: {
    currentYear() {
      return new Date().getFullYear();
    }
  }
}
</script>

<style lang="scss" scoped>
.footer {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 20px 20px 0 0;
  box-shadow: 0 -4px 6px rgba(0, 0, 0, 0.1);

  .footer-link {
    color: rgba(255, 255, 255, 0.8);
    text-decoration: none;
    transition: all 0.3s ease;
    padding: 0.5rem 1rem;
    border-radius: 20px;
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;

    i {
      font-size: 1.1rem;
    }

    &:hover {
      color: white;
      background: rgba(255, 255, 255, 0.1);
      transform: translateY(-2px);
    }
  }

  .list-inline-item {
    margin: 0 0.5rem;
  }
}
</style>