<template>
  <div class="layout-dashboard" :class="{ blurred: blurred }">
    <aside>
      <nuxt-link to="/home">
        <img src="~/assets/icons/casa.svg">

        Home
      </nuxt-link>

      <nuxt-link to="/bitcoin">
        <img src="~/assets/icons/bitcoin.svg">

        Bitcoin
      </nuxt-link>

      <nuxt-link to="/lightning">
        <img src="~/assets/icons/lightning.svg">

        Lightning
      </nuxt-link>

      <!--
      <nuxt-link to="/btcpay">
        BTCPay Server
      </nuxt-link>
      -->

      <nuxt-link to="/system">
        <img src="~/assets/icons/system.svg">

        System
      </nuxt-link>
    </aside>

    <main>
      <div class="wrapper">
        <nuxt />

        <template v-if="activeModal">
          <component :is="activeModal" :props="props"/>
        </template>
      </div>
    </main>
  </div>
</template>

<script>
  import Events from '~/helpers/events';
  import UnlockModal from '~/components/system/modals/Unlock';
  import Welcome from '~/components/system/modals/Welcome';

  export default {
    data() {
      return {
        activeModal: false,
        blurred: false,
        props: {},
      }
    },

    created() {
      Events.$on('modal-open', (modal, props) => {
        this.activeModal = modal;

        // Default props to empty object to avoid props from one modal being passed to the next modal.
        this.props = props || undefined;
        this.blurred = true;
      });

      Events.$on('modal-close', () => {
        this.activeModal = false;
        this.blurred = false;
      });

      Events.$on('unlock-modal-open', () => {
        this.activeModal = UnlockModal;
        this.blurred = true;
      });

      // Check for and clear welcome boolean
      if(localStorage.getItem('welcome') === 'true') {
        this.activeModal = Welcome;
        this.blurred = true;
        localStorage.removeItem('welcome');
      }

    },

    destroyed() {
      Events.$off('modal-opened');
      Events.$off('modal-closed');
    }
  }
</script>

<style lang="scss">
  @import "~/assets/css/global.scss";

  .layout-dashboard {
    display: flex;
    min-height: 100vh;

    &.blurred {
      aside, .page {
        filter: blur(8px);
      }
    }

    aside {
      width: 250px;
      border-right: 2px solid $transparentWhite;
      padding: 3em;

      a {
        color: $gray;
        display: block;
        font-weight: bold;
        margin-bottom: 2.5em;
        outline: none;
        white-space: nowrap;
      }

      img {
        vertical-align: middle;
        margin-right: 1em;
      }

      .nuxt-link-active {
        color: $purple;
      }
    }

    main {
      padding: 3em 6em 4em 4em;
      flex-grow: 1;

      .page {
        position: relative;
      }
    }
  }
</style>
