<script setup>
const firebaseConfig = {
  apiKey: "AIzaSyCOBzkwEZr4fMcrfJnhPtFfU3qSD2tW_d4",
  authDomain: "jemeapp-subdomains.firebaseapp.com",
  projectId: "jemeapp-subdomains",
  storageBucket: "jemeapp-subdomains.firebasestorage.app",
  messagingSenderId: "727753249838",
  appId: "1:727753249838:web:35521bd80ba83267e13bbf",
  measurementId: "G-K6ZYKCPLTH"
};
</script>

<template>
  <h1>Create redirect</h1>
  <form>
    <label for="url">URL:</label>
    <input type="text" id="url" name="url">
    <input type="submit" value="Create">
  </form>
</template>
