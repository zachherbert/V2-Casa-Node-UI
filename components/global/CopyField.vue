<template>
  <div class="copy-field">
    <div class="flex">
      <span class="numeric centered-vertically copy-content">{{ value }}</span>
      <a class="button is-small" @click="copy()">Copy</a>
    </div>
  </div>
</template>

<script>

  import {copyToClipboard} from '@/helpers/utils';

  export default {
    props: {
      'value': {
        default: 'Loading...',
        type: String,
      },
    },

    methods: {
      copy() {
        copyToClipboard(this.value);
        this.$toasted.global.default({ message: 'Copied to clipboard.' });
      }
    }
  }
</script>

<style lang="scss">
  @import "~/assets/css/variables.scss";

  .copy-field {
    display: inline-block;
    border-radius: 32.5px;
    border: dashed 2px $purple;
    background-color: $transparentFaintWhite;

    .flex {
      margin: 0.1em;
      margin-left: 1.5em;
    }

    .button {
      margin-right: 0.55em;
      margin-left: auto;
      padding: 0 2em;
      
      &:active {
        border: 3px solid #FFF!important;
      }
      &:hover {
        border: 3px solid #865efc;
      }
    }

    .copy-content {
      overflow: hidden;
      text-overflow: ellipsis;
      padding-right: 1em;
    }

    input {
      position: fixed;
      height: 0;
      width: 0;
      opacity: 0;
      z-index: -1;
    }
  }
</style>
