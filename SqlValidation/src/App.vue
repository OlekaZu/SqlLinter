<template>
  <!-- <div>
    <SqlEditor :code="code" />
  </div> -->
  <div>
    <NewMonacoEditor />
  </div>
</template>

<script setup lang="ts">
// import TextInput from "./components/TextInput.vue";
import SqlEditor from "./components/SqlEditor.vue";
import MonacoEditor from "./components/MonacoEditor.vue";
import NewMonacoEditor from "./components/NewMonacoEditor.vue";
</script>
