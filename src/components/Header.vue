<template>
  <header class="header">
    <nav class="header__nav">
      <router-link
        :to="{name: 'home'}"
        class="header__nav__link"
        active-class="header__nav__link-active"
        exact>
        Home
      </router-link>
      <router-link
        to="edit" class="header__nav__link"
        active-class="header__nav__link-active"
        exact>
        Add user
      </router-link>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'main-header',
};
</script>

<style scoped lang="scss">
  .header {
    width: 100%;
    background: #2e7d32;
    &__nav {
      width: 1024px;
      margin: 0 auto;
      display: flex;
      &__link {
        padding: 10px;
        text-decoration: none;
        color: #ffffff;
        &-active {
          background: #ffffff;
          color: #2e7d32;
        }
      }
    }
  }
</style>
