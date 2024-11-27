<script setup>
import { ref, onMounted } from "vue";

const title = ref("");
const url = ref("");
const cp_raw = ref("Copy Raw")
const cp_mkdwn = ref("Copy Markdown")

onMounted(() => {
  chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    if (tabs.length > 0) {
      title.value = tabs[0].title || "No Title";
      url.value = tabs[0].url || "No URL";
    }
  });
});

const copy_raw = () => {
  const to_copy_raw = `${title.value} : ${url.value}`;
  navigator.clipboard.writeText(to_copy_raw).then(() => {
    cp_raw.value = "Copied 🎉"
  });
};

const copy_mkdwn = () => {
  const to_copy_markdown = `[${title.value}](${url.value})`;
  navigator.clipboard.writeText(to_copy_markdown).then(() => {
    cp_mkdwn.value = "Copied 🎉"
  });
};

</script>

<template>
  <div>
    <h4>Title: {{ title }}</h4>
    <h4>URL:</h4><p><a :href="url"> {{ url }}</a></p>
  </div>
  <div>
    <button @click="copy_raw">{{ cp_raw }}</button> <button @click="copy_mkdwn">{{ cp_mkdwn }}</button>
  </div>
</template>
