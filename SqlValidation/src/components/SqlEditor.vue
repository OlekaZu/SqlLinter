<template>
  <div class="editor">
    <codemirror
      v-model="code"
      :style="{
        width: 'auto',
        height: 'auto',
        backgroundColor: '#fff',
        color: '#333',
      }"
      placeholder="Please enter the code."
      :extensions="extension"
      :autofocus="true"
      :disabled="false"
      :indent-with-tab="true"
      :tab-size="4"
      lint="true"
    />
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import { Codemirror } from "vue-codemirror";
import { sql, PostgreSQL } from "@codemirror/lang-sql";
import { oneDark } from "@codemirror/theme-one-dark";

const code = ref(`SELEC d.nzp_dom, da.nzp_da, da.nom, da.type_apart
  FROM bill_data.dom d
  JOIN bill_data.dom_apartment da ON da.nzp_dom = d.nzp_dom AND da.is_actual
WHERE d.is_actual AND d.nzp_dom = 901356338 AND da.nzp_da = 906764915
  AND da.type_apart IN (1, 2)`);

const extension = [sql({ dialect: PostgreSQL }), oneDark];

onMounted(() => {});
</script>

<style scoped>
.editor {
  display: flex;
  align-items: top;
  width: 500px;
  font-family: Consolas, Monaco, "Courier New", monospace;
  font-size: 14px;
  height: 500px;
  text-align: left;
}
</style>
