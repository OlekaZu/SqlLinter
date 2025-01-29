<template>
  <div class="editor">
    <div class="main">
      <codemirror
        v-model="code"
        :style="{
          width: '100%',
          height: '100%',
          backgroundColor: '#fff',
          color: '#333',
        }"
        placeholder="Please enter the code."
        :extensions="extension"
        :autofocus="true"
        :disabled="false"
        :indent-with-tab="true"
        :tab-size="2"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, watch, shallowRef } from "vue";
import { Codemirror } from "vue-codemirror";
import { sql, PostgreSQL } from "@codemirror/lang-sql";

// theme css
// import "codemirror/theme/solarized.css";

const props = defineProps<{
  code: string;
}>();
const code = shallowRef(props.code);

const extension = [sql({ dialect: PostgreSQL })];

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
  .divider {
    height: 1px;
    background-color: white;
  }

  .main {
    display: flex;
    width: 100%;
  }
}
</style>
