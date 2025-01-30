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
import { onMounted, watch, shallowRef } from "vue";
import { Codemirror } from "vue-codemirror";
import { sql, PostgreSQL } from "@codemirror/lang-sql";
import { oneDark } from "@codemirror/theme-one-dark";

const props = defineProps<{
  code: string;
}>();
const code = shallowRef(props.code);

const extension = [sql({ dialect: PostgreSQL }), oneDark];

onMounted(() => {
  watch(
    () => props.code,
    (_code) => {
      code.value = _code;
    }
  );
});
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
