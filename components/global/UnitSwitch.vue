<template>
  <div class="unit-switch">
    <label class="toggle">
      <input type="checkbox" :checked="displayUnit === 'sats'" @change="toggle">
      <span class="toggle-slider" />

      <div class="toggle-options">
        <div class="toggle-option one" :class="{active: displayUnit === 'btc'}">
          BTC
        </div>

        <div class="toggle-option two" :class="{active: displayUnit === 'sats'}">
          SATS
        </div>
      </div>
    </label>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  export default {
    computed: {
      ...mapGetters({ displayUnit: 'system/getUnits' })
    },

    methods: {
      async toggle() {
        if(this.displayUnit === 'btc') {
          this.$store.dispatch('system/setUnits', 'sats');
        } else {
          this.$store.dispatch('system/setUnits', 'btc');
        }
      },
    }
  };
</script>

<style lang="scss">
  @import "~/assets/css/variables.scss";

  .unit-switch {
    position: absolute;
    right: 0;
    top: 0.75em;
  }

  /* Toggle switch buttons */
  .toggle {
    position: relative;
    display: inline-block;
    width: 128px;
    height: 35px;
    max-width: 100%;
    font-size: 14px;
    font-weight: 900;
  }

  .toggle input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .toggle-slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border: 3px solid $blue;
    transition: 0.5s;
    border-radius: 20px;
  }

  .toggle-slider:before {
    position: absolute;
    content: "";
    height: 100%;
    width: 50%;
    left: 0;
    top: -3px;
    left: -1px;
    background-color: $blue;
    transition: 0.5s;
    border: 3px solid $blue;
    box-sizing: content-box;
    border-radius: 20px;
  }

  .toggle input:checked + .toggle-slider:before {
    transform: translateX(calc(100% - 7px));
  }

  .toggle-options {
      display: flex;
      text-decoration: none;
  }

  .toggle-option {
      position: relative;
      z-index: 1;
      cursor: pointer;
      top: -1em;
      transition: 0.5s;
      transition-property: font-weight, color;
      width: 50%;
      text-align: center;
      font-weight: bold;
      padding-left: 0.5em;
      font-size: 13px;
      letter-spacing: 1px;
  }

  .toggle-option.two {
    padding-right: 0.5em;
  }

  .toggle-option.active {
      transition-delay: 0.2s;
      color: #fff;
  }
</style>