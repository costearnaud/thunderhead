<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

defineProps<{ msg: string }>();

const count = ref(0);
const urlSFMCToken = ref('https://mc4m3gyjn-56t5p2511h2mb76-xq.auth.marketingcloudapis.com/v2/token');
const urlISToken = ref('https://eu2.thunderhead.com/one/oauth2token');
const payload = {
  grant_type: "client_credentials",
  client_id: "pm3vo9fzp81benu54s71hksn",
  client_secret: "k4zweavfyQcMGaYw0oqog2iE",
  account_id: "7213695"
};
const payloadIS = 'grant_type=client_credentials';

// Request SFMC Token
const getToken = async (body:object) => {
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

// Request IS Token
const getTokenIS = async (body:string) => {
  try {
    const res = await axios.post(urlISToken.value, body, {
      headers: {
        'Authorization': 'Basic ZmUxMWY3NzMtNjZmYi00ZjIwLTk5NDgtODJmYWU2MTc1MjEzOjk5MWRlNTYwLTZmZjgtNDcyOS1hMGIxLWU1OWNlYjExOTk0OA==',
        'content-type': 'application/x-www-form-urlencoded',
        'Accept': 'application/json'
      }
    })
    return res.data
  } catch (err) {
    console.log('getTokenIS error : ' + err);
    return (err);
  }
};

//const token = getToken (payload);
const tokenISPromise = getTokenIS (payloadIS);
const tokenPromise = getToken(payload);

let tokenIS = '';
tokenISPromise.then((value) => {
  tokenIS = value.access_token;
  console.log('Token IS : ', tokenIS);
});

let token = '';
tokenPromise.then((value) => {
  token = value.access_token;
  console.log('Token SFMC : ', token);
});

const DataStore = 'IS_Priority_BTS_082021';
const workspace = 'MjlwNTEz';
const env = 'live';
const urlDataStore = 'https://eu2.thunderhead.com/datastores/1.0.0/'+workspace+'/'+DataStore;

// Delete one DataStore
const deleteDataStore = async () => {
  try {
    const res = await axios.delete(urlDataStore, {
      headers: {
        'Authorization': 'Bearer '+tokenIS,
        'content-type': 'application/x-www-form-urlencoded',
        'Accept': '*/*'
      }
    })
    return res.data
  } catch (err) {
    console.log('deleteDataStore error : ' + err)
    return (err)
  }
};

const deletePromise = deleteDataStore();
deletePromise.then((value) => {
  console.log('Retour delete : ', value);
});

</script>

<template>
  <h1>{{ msg }}</h1>

  <p>
    Recommended IDE setup :
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
