<script setup>
import { ref, onMounted } from "vue";
import { db } from "@/firebase";
import { useRoute } from "vue-router"; 
import { getDoc, doc } from "firebase/firestore"

const route = useRoute();

const redirectUrl = ref("");
const isError = ref(false);

async function fetchAndRedirect() {
  const id = route.params.id.toLowerCase();

  try {
    const docSnap = await getDoc(doc(db, "redirects", id));
    if (docSnap.exists()) {
      redirectUrl.value = docSnap.data().url;
      window.location.href = redirectUrl.value;
    } else {
      isError.value = true;
    }
  } catch (error) {
    isError.value = true;
  }
}

onMounted(() => {
  fetchAndRedirect();
});
</script>

<template>
  <main>
    <h1 v-if="!isError">Redirecting...</h1>
    <div v-if="isError">
      <h1>404</h1>
      <p>Redirect not found</p>
      <a href="/">Go back to homepage</a>
    </div>
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
</style>
