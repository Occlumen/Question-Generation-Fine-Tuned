<template>
  <div class="container">
    <h1>Question Generation</h1>
    <form @submit.prevent="submitForm">
      <div class="upload-section">
        <input type="file" accept="application/pdf" @change="e => selectedFile = e.target.files[0]" />
        <button type="submit" :disabled="loading">
          <span v-if="!loading">Upload</span>
        </button>
      </div>
    </form>
    <div v-if="loading" class="loading-overlay">
      <div class="loading-spinner"></div>
    </div>
    <div v-else class="content-section">
      <div class="left-section">
        <h2>Topic.json</h2>
        <p v-for="(value, key) in topicJson" :key="key">
          {{ key }}: {{ value }}
        </p>
      </div>
      <div class="right-section">
        <h2>Questions.json</h2>
        <p v-for="(value, key) in questionJson" :key="key">
          {{ key }}: {{ value }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const loading = ref(false);
const selectedFile = ref(null);

const topicJson = ref("");
const questionJson = ref("");

const submitForm = () => {
  if (selectedFile.value) {
    console.log(selectedFile.value);
    loading.value = true;
    let file = selectedFile.value;
    let formData = new FormData();
    formData.append('file', file);
    console.log("fetch");
    const apiUrl = process.env.VUE_APP_QUESTION_GENERATION;
    console.log("API URL:", apiUrl);
    fetch(apiUrl, {
      method: 'POST',
      body: formData
      })
      .then(resp => resp.json())
      .then(data => {
          if (data.errors) {
            alert(data.errors)
          }
          else {
            loading.value = false;
            console.log(data);
            topicJson.value = data.topic_json;
            questionJson.value = data.question_json;
          }
      })
  } else {
    alert("Please select a file first.");
  }
};

</script>

<style scoped>
/* General styling */
.container {
  text-align: center;
  margin: 0 auto;
  width: 80%;
  height: 80%;
  position: relative;
  background-color: #f8f8f8; /* Light beige background */
  border: 2px solid #ccc; /* Light gray border */
  border-radius: 12px;
  padding: 30px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: #333; /* Neutral gray */
}

.upload-section {
  margin-bottom: 30px;
}

input[type="file"] {
  margin-bottom: 10px;
}

button {
  background-color: #fff; /* White background */
  border: 1px solid #999; /* Gray border */
  color: #555; /* Dark gray text */
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Light shadow */
  transition: background-color 0.3s, color 0.3s;
}

button:hover {
  background-color: #f0f0f0; /* Slightly darker shade */
  color: #333; /* Darker text */
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

/* Loader styling */
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.9); /* Neutral overlay */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 5px solid #ddd; /* Neutral border */
  border-top: 5px solid #555; /* Dark gray */
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* Content section styling */
.content-section {
  display: flex;
  gap: 20px;
  justify-content: space-between;
  height: 100%; /* Take up available height */
  overflow: hidden; /* Prevent content from spilling out */
}

.left-section,
.right-section {
  flex: 1;
  max-height: 100%; /* Ensure sections don't exceed container height */
  background-color: #fff; /* White background */
  border: 1px solid #ccc; /* Light gray border */
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Light shadow */
  overflow-y: auto; /* Add vertical scrolling for content overflow */
  text-align: left; /* Align text to the left */
}

h2 {
  font-size: 1.5rem;
  color: #444; /* Neutral dark gray */
  margin-bottom: 10px;
  text-align: left; /* Ensure headings are also left-aligned */
}

p {
  margin-bottom: 10px;
  line-height: 1.6; /* Adjust line height for readability */
  text-align: left; /* Align paragraph text to the left */
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>


