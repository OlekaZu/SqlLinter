<template>
  <div class="editor">
    <vue-monaco-editor
      ref="editorRef"
      v-model:value="code"
      theme="vs"
      :options="MONACO_EDITOR_OPTIONS"
      language="sql"
      @mount="handleMount"
      @change="onChange"
      @validate="
        (errors) => {
          monaco.editor.setModelMarkers(editorRef.getModel(), 'sql', errors);
        }
      "
    />
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, shallowRef } from "vue";
import { VueMonacoEditor, type MonacoEditor } from "@guolao/vue-monaco-editor";
import * as monaco from "monaco-editor";
import {
  LanguageIdEnum,
  setupLanguageFeatures,
  vsPlusTheme,
} from "monaco-sql-languages";
import { PostgreSQL } from "dt-sql-parser";
import "monaco-sql-languages/esm/languages/pgsql/pgsql.contribution";
import { Parser } from "node-sql-parser";

const code = ref(`SELEC d.nzp_dom, da.nzp_da, da.nom, da.type_apart
  FROM bill_data.dom d
  JOIN bill_data.dom_apartment da ON da.nzp_dom = d.nzp_dom AND da.is_actual
WHERE d.is_actual AND d.nzp_dom = 901356338 AND da.nzp_da = 906764915
  AND da.type_apart IN (1, 2)`);

const editorRef = shallowRef();

const handleMount = (editorInstance) => {
  editorRef.value = editorInstance;
  validateAndHighlLight(code.value);
};
// const handleMount = () => {
//   const editor = monaco.editor.create(editorRef.value);
//   modelRef.value = editor.getModel();
// };

const MONACO_EDITOR_OPTIONS = {
  automaticLayout: true,
  formatOnType: true,
  formatOnPaste: true,
  minimap: { enabled: false },
  defaultColorDecorators: true,
  colorDecorators: true,
  lineHeight: 30,
  inlineHints: { enabled: "on" },
};

const parserOpt = {
  database: "Postgresql",
};

// const lang = LanguageIdEnum.PG;

// setupLanguageFeatures(LanguageIdEnum.PG, {
//   completionItems: {
//     enable: true,
//     triggerCharacters: [" ", "."],
//   },
// });

const pgsql = new PostgreSQL();
const onChange = (newCode: string) => {
  validateAndHighlLight(newCode);
};

function validateAndHighlLight(newCode: string) {
  const errors = validateSql(newCode);
  // console.log(errors);
  const model = editorRef.value?.getModel();
  // console.log(model);
  monaco.editor.setModelMarkers(model, "sql", errors);
  const markers = monaco.editor.getModelMarkers(model);
  // console.log(markers);
}

function validateSql(newCode: string) {
  const errors = pgsql.validate(newCode);
  // console.log(errors);

  const errorsRequered = errors.map((item) => ({
    message: item.message,
    severity: monaco.MarkerSeverity.Error,
    startLineNumber: item.startLine,
    startColumn: item.startColumn,
    endLineNumber: item.endLine,
    endColumn: item.endColumn,
  }));
  return errorsRequered;
}

// your action
function formatCode() {
  editorRef.value?.getAction("editor.action.formatDocument").run();
}

onMounted(() => {
  // const parser = new Parser();
  // const ast = parser.astify(code.value, parserOpt);
  // console.log(ast);
});
</script>

<style scoped>
.editor {
  display: flex;
  /* align-items: top; */
  width: 700px;
  /* font-family: Consolas, Monaco, "Courier New", monospace;
  font-size: 20px; */
  height: 500px;
  text-align: left;
}
</style>
