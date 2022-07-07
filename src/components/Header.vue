<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <a class="navbar-item" href="#">
        <img alt="Vue logo" src="../assets/logo.png" width="34" height="34" />
        &nbsp;
        <label class="is-size-4" for="brand">VueJs</label>
      </a>

      <a
        role="button"
        class="navbar-burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarBasicExample"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
      <div class="navbar-start">
        <a class="navbar-item"> Home </a>
      </div>

      <div class="navbar-end">
        <div class="navbar-item"></div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Header",
};
</script>
