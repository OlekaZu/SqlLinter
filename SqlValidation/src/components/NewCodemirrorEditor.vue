<template>
  <codemirror
    class="editor"
    v-model="sqlCode"
    :options="cmOptions"
    :extensions="extension"
  />
</template>

<script>
import { Codemirror } from "vue-codemirror";
import { sql, PostgreSQL } from "@codemirror/lang-sql";
// import "codemirror/mode/sql/sql";
// import "codemirror/lib/codemirror.css";

export default {
  components: { Codemirror },
  data() {
    return {
      sqlCode: `SELEC d.nzp_dom, da.nzp_da, da.nom, da.type_apart
  FROM bill_data.dom d
  JOIN bill_data.dom_apartment da ON da.nzp_dom = d.nzp_dom AND da.is_actual
WHERE d.is_actual AND d.nzp_dom = 901356338 AND da.nzp_da = 906764915
  AND da.type_apart IN (1, 2)`,
      cmOptions: {
        mode: "sql",
        lineNumbers: true,
      },
      extension: [sql({ dialect: PostgreSQL })],
    };
  },
};
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
