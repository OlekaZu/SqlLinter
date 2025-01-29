<template>
  <div class="text-input-component">
    <textarea
      v-model="inputText"
      rows="10"
      cols="50"
      placeholder="Введите текст..."
    ></textarea>
    <p>Вы ввели: {{ inputText }}</p>
    <SqlEditor :code="code" />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import SqlEditor from "./SqlEditor.vue";
// import { EditorView, basicSetup } from "codemirror";
// import { sql, PostgreSQL } from "@codemirror/lang-sql";

// const view = new EditorView({
//   parent: document.body,
//   doc: `select * from users where age > 20`,
//   extensions: [basicSetup, sql({ dialect: PostgreSQL })],
// });

const code = ref<string>("select * from users where age > 20");

const inputText = ref("");

onMounted(() => {});
</script>

<style scoped>
.text-input-component {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

textarea {
  border-radius: 5px;
  border: 1px solid #ccc;
}
</style>
