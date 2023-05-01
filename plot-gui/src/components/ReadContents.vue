<script setup>
import { ref } from "vue";
import { open } from "@tauri-apps/api/shell";
import { readTextFile } from "@tauri-apps/api/fs";

const contents = ref("");

const readFileContents = async () => {
  try {
    const selectedPath = await open({
      multiple: false,
      title: "Select a file",
    });
    if (!selectedPath) {
      return;
    }
    contents.value = await readTextFile(String(selectedPath));
  } catch (error) {
    console.error(error);
  }
};

</script>

<template>
    <p v-if="contents.length">{{ contents }}</p>
    <button type="button" @click="readFileContents()">Read File</button>
</template>