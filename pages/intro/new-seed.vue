<template>
  <div class="intro-new-seed">
    <main>
      <div v-if="count > 0" class="count">
        {{ count }}
      </div>

      <div class="seed">
        {{ seed }}
      </div>
    </main>

    <footer>
      <a class="button" @click="previousSeed()">
        Go Back
      </a>

      <a class="button is-primary" :disabled="seedPhrase.length === 0" @click="nextSeed()">
        Next
      </a>
    </footer>
  </div>
</template>

<script>
  import API from '@/helpers/api';
  import {sleep} from '@/helpers/utils';

  export default {
    data() {
      return {
        count: 0,
        seed: 'Loading...',
        seedPhrase: [],
      }
    },

    async created() {
      let data = false;

      // Keep trying to get the seed, in case lnd is still starting
      while(data === false) {
        data = await API.get(this.$axios, `${this.$env.API_LND}/v1/lnd/wallet/seed`);

        if(data === false) {
          // Todo: Output error message from 500 error?
          await sleep(5000);
        }
      }

      if(data && data.seed.length === 24) {
        this.count = 1;
        this.seedPhrase = data.seed;
        this.displaySeed();
      }

      // Todo: Display an error message if the seed phrase fails to load?
      // Todo: Display a loading indicator instead of the text "Loading..."?
    },

    methods: {
      displaySeed() {
        this.seed = this.seedPhrase[this.count - 1];
      },

      previousSeed() {
        if(this.count === 1) {
          this.$router.push({ path: '/intro/seed' });
        } else {
          this.count--;
          this.displaySeed();
        }
      },

      nextSeed() {
        if(this.seedPhrase.length === 0) {
          return;
        } else if(this.count === this.seedPhrase.length) {
          // We have to use the route name here instead of the path, otherwise the params won't be passed (a weird quirk of Vue router?)
          this.$router.push({ name: 'intro-password', params: { seedPhrase: this.seedPhrase }});
        } else {
          this.count++;
          this.displaySeed();
        }
      },
    },
  }
</script>
<style lang="scss">
  @import "~/assets/css/variables.scss";

  .intro-new-seed {
    .count {
      background-color: $darkPurple;
      border-radius: 100%;
      font-size: 36px;
      font-weight: 900;
      width: 2.5em;
      height: 2.5em;
      padding-top: 0.5em;
      margin: 1em auto;
    }

    .seed {
      background-color: $black;
      font-size: 60px;
      font-weight: 500;
      margin: 0.75em auto;
      padding: 0.5em 2em;
      border-radius: 80px;
    }
  }
</style>
