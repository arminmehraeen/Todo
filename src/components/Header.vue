<template>
  <header class="header py-4">
    <div class="container">
      <div class="row align-items-center">
        <div class="col-md-6 text-center text-md-end">
          <h1 class="display-4 mb-2">برنامه لیست کارها</h1>
          <p class="lead text-muted">
            این برنامه با استفاده از ویو جی اس ساخته شده است. امیدوارم از آن لذت ببرید
          </p>
        </div>
        <div class="col-md-6 text-center text-md-start">
          <img src="./../assets/logo.png" 
               alt="Vue logo" 
               class="header-logo animate__animated animate__bounceIn">
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style lang="scss" scoped>
.header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  margin-bottom: 2rem;
  border-radius: 0 0 20px 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

  .header-logo {
    max-width: 150px;
    height: auto;
    filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.3));
    transition: transform 0.3s ease;

    &:hover {
      transform: scale(1.05);
    }
  }

  h1 {
    font-weight: 700;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  }

  .lead {
    opacity: 0.9;
  }
}
</style>