<template>
  <div ref="editorContainer" class="editor"></div>
</template>

<script>
import { ref, onMounted } from "vue";
import * as monaco from "monaco-editor";
import { PostgreSQL } from "dt-sql-parser";

export default {
  setup() {
    const editorContainer = ref(null);
    const pgsql = new PostgreSQL();
    const errors = [];

    onMounted(() => {
      const editor = monaco.editor.create(editorContainer.value, {
        value: `SELECT d.nzp_dom, da.nzp_da, da.nom, da.type_apart FROM bill_data.dom d
JOIN bill_data.dom_apartment da ON da.nzp_dom = d.nzp_dom AND da.is_actual
WHERE d.is_actual AND d.nzp_dom = 901356338 AND da.nzp_da = 906764915
  AND da.type_apart IN (1, 2`, // Invalid SQL for testing
        language: "sql",
        theme: "vs", // Use light theme for better visibility,
        minimap: { enabled: false }, // Disable minimap
      });

      // Validate SQL on content change
      editor.onDidChangeModelContent(() => {
        validateAndHighlight(editor);
      });

      // Initial validation
      validateAndHighlight(editor);
    });

    // Custom SQL Validation Function
    function validateSQL(sql) {
      const errorsParse = pgsql.validate(sql);
      if (errorsParse.length > 0) {
        errorsParse.forEach((x) =>
          errors.push({
            message: x.message,
            severity: monaco.MarkerSeverity.Error,
            startLineNumber: x.startLine,
            startColumn: x.startColumn,
            endLineNumber: x.endLine,
            endColumn: x.endColumn,
          })
        );
      }
      //   if (!sql.includes("SELECT")) {
      //     errors.push({
      //       message: "SQL query must contain a SELECT statement.",
      //       severity: monaco.MarkerSeverity.Error,
      //       startLineNumber: 1,
      //       startColumn: 1,
      //       endLineNumber: 1,
      //       endColumn: 10, // Fixed range for testing
      //     });
      //   }
      return errors;
    }

    // Validate and Highlight Errors
    function validateAndHighlight(editor) {
      const model = editor.getModel();
      const sql = model.getValue();
      const errors = validateSQL(sql);

      console.log("Model:", model); // Debugging
      console.log("Errors:", errors); // Debugging

      // Set markers for errors
      monaco.editor.setModelMarkers(model, "sql", errors);

      // Log markers to verify they are applied
      const markers = monaco.editor.getModelMarkers(model);
      console.log("Markers:", markers); // Debugging
    }

    return {
      editorContainer,
    };
  },
};
</script>

<style scoped>
.editor {
  border: 1px solid #ccc;
  display: flex;
  align-items: top;
  width: 700px;
  height: 500px;
  text-align: left;
}
</style>
