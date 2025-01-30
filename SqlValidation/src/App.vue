<template>
  <div>
    <SqlEditor :code="code" />
  </div>
</template>

<script setup lang="ts">
// import TextInput from "./components/TextInput.vue";
import SqlEditor from "./components/SqlEditor.vue";

const code = `SELECT d.nzp_dom, da.nzp_da, da.nom, da.type_apart 
  FROM bill_data.dom d
  JOIN bill_data.dom_apartment da ON da.nzp_dom = d.nzp_dom AND da.is_actual
WHERE d.is_actual AND d.nzp_dom = 901356338 AND da.nzp_da = 906764915
  AND da.type_apart IN (1, 2)`;
</script>
