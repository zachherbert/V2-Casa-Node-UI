<template>
  <div class="casa-footer">
    <img src="~/assets/icons/casa-colorized.svg">

    <a href="https://keys.casa/" target="_blank" rel="noopener">Home</a>
    <span class="separator" />
    <a href="https://keys.casa/support/" target="_blank" rel="noopener">Support</a>
    <span class="separator" />
    <a href="https://keys.casa/legal/" target="_blank" rel="noopener">Legal</a>
  </div>
</template>

<style lang="scss">
  @import "~/assets/css/variables.scss";

  .casa-footer {
    margin-bottom: 3em;
    font-size: 15px;

    img {
      display: block;
      margin: 1.5em auto;
    }

    a {
      color: $gray;
    }
  }
</style>
