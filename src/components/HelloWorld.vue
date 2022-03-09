<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

defineProps<{ msg: string }>();

const count = ref(0);
const urlSFMCToken = ref('https://mc4m3gyjn-56t5p2511h2mb76-xq.auth.marketingcloudapis.com/v2/token');

const payload = {
  grant_type: "client_credentials",
  client_id: "pm3vo9fzp81benu54s71hksn",
  client_secret: "k4zweavfyQcMGaYw0oqog2iE",
  account_id: "7211327"
};

// Request SFMC Token
const getToken = async (body:{}) => {
  try {
    const res = await axios.post(urlSFMCToken.value, body, {
      headers: {
        'content-type': 'text/json',
        'Accept': '*/*'
      }
    })
    return res.data
  } catch (err) {
    console.log('getToken error : ' + err)
    return (err)
  }
};

const token = getToken (payload);
console.log(token);

</script>

<template>
  <h1>{{ msg }}</h1>

  <p>
    Recommended IDE setup v2:
    <a href="https://code.visualstudio.com/" target="_blank">VSCode</a>
    +
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
  </p>

  <p>See <code>README.md</code> for more information.</p>

  <p>
    <a href="https://vitejs.dev/guide/features.html" target="_blank">
      Vite Docs
    </a>
    |
    <a href="https://v3.vuejs.org/" target="_blank">Vue 3 Docs</a>
  </p>

  <button type="button" @click="count++">count is: {{ count }}</button>
  <p>
    Edit
    <code>components/HelloWorld.vue</code> to test hot module replacement.
  </p>
</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
