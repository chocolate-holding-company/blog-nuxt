<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <NuxtLink to="/" class="logo">
          <h1>Hill Walking Made Easy</h1>
        </NuxtLink>
        <nav class="nav">
          <NuxtLink to="/" class="nav-link">Home</NuxtLink>
        </nav>
      </div>
    </div>
  </header>
</template>

<style lang="scss" scoped>
.header {
  background-color: $primary-color;
  color: $text-light;
  padding: $spacing-md 0;
  box-shadow: 0 2px 8px $shadow;
  position: sticky;
  top: 0;
  z-index: 100;

  .header-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    h1 {
      font-size: 1.5rem;
      margin: 0;
      color: $text-light;
      transition: $transition;

      &:hover {
        opacity: 0.9;
      }

      @media (max-width: $breakpoint-mobile) {
        font-size: 1.2rem;
      }
    }
  }

  .nav {
    display: flex;
    gap: $spacing-md;

    .nav-link {
      color: $text-light;
      padding: $spacing-xs $spacing-sm;
      border-radius: $border-radius;
      transition: $transition;

      &:hover {
        background-color: rgba(255, 255, 255, 0.1);
      }
    }
  }
}
</style>
