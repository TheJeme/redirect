<script setup>
import { ref } from "vue";
import { db } from "@/firebase";
import { setDoc, doc } from "firebase/firestore"
import { customAlphabet } from 'nanoid'
const nanoid = customAlphabet('abcdefghkmnopqrstuvwxyz', 5)

const url = ref("");
const id = ref("");
const isInvalidUrl = ref(false);

function formatUrl(url) {
  const urlPattern = /^(https?:\/\/)?([\w-]+\.)+[\w-]{2,}(\/\S*)?$/;

  if (urlPattern.test(url)) {
    return url.startsWith("http") ? url : `https://${url}`;
  }
  return null;
}

async function createRedirect() {
  const formattedUrl = formatUrl(url.value);
  
  if (!formattedUrl) {
    isInvalidUrl.value = true;
    return;
  }
  isInvalidUrl.value = false;

  const _id = nanoid()
  try {
    await setDoc(doc(db, "redirects", _id), {
      url: formattedUrl
    });

    url.value = "";
    id.value = _id
  } catch (error) {
    console.error("Skill issue:", error);
  }
};
</script>

<template>
  <main>
    <h1>Create redirect</h1>
    <form @submit.prevent="createRedirect">
      <input type="text" id="url" name="url" placeholder="URL" v-model="url">
      <input type="submit" value="Create">
    </form>
    <p v-if="isInvalidUrl">Invalid URL</p>
    <a :href="`https://r.jeme.app/${id}`" target="_blank" rel="noopener noreferrer" v-if="id">https://r.jeme.app/{{ id }}</a>
  </main>
</template>

<style scoped>
main {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  width: 100%;
  font-size: 18px;
  margin: 0 auto;
  text-align: center;
}

p {
  color: red;
}

form {
  display: flex;
  gap: 1rem;
  margin: 0 auto;
  justify-content: center;
}

#url {
  width: 100%;
  max-width: 220px;
}

</style>