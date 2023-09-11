<template>
  <div>
    <div class="editor-toolbar">
      <button @click="applyStyle('bold')" class="size">B</button>
      <button @click="applyStyle('italic')" class="size">I</button>
      <button @click="applyAlignment('left')" class="size">Left</button>
      <button @click="applyAlignment('center')" class="size">Center</button>
      <button @click="applyAlignment('right')" class="size">Right</button>
      <button v-for="size in fontSize" :key="size" @click="increaseFontSize(size)" class="size">{{ size }}</button>
    </div>
    <div class="editor">
      <div class="edit-area" ref="editable" contenteditable="true" @input="updateText" :style="textStyles"></div>
    </div>
    <button @click="checkText">Тестануть</button>
    <div v-html="result" v-if="result !== null"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputText: "", // Введенный текст
      bold: false, // Флаг для жирного шрифта
      italic: false, // Флаг для курсива
      alignment: "left", // Выравнивание текста,
      result: null,
      fontSize: [2, 3, 4, 5, 6, 7]
    };
  },
  computed: {
    textStyles() {
      let styles = "";
      if (this.bold) {
        styles += "font-weight: bold;";
      }
      if (this.italic) {
        styles += "font-style: italic;";
      }
      if (this.alignment === "center") {
        styles += "text-align: center;";
      } else if (this.alignment === "right") {
        styles += "text-align: right;";
      }
      return styles;
    }
  },
  methods: {
    applyStyle(style) {
      // Применение стилей к выделенному тексту
      document.execCommand(style, false, null);
    },
    applyAlignment(align) {
      // Применение выравнивания текста
      document.execCommand("justify" + align);
    },
    updateText() {
      // Обновление текста при вводе
      this.inputText = this.$refs.editable.innerText;
    },

    increaseFontSize(size) {
      // Увеличение размера шрифта текущего выделенного текста
      document.execCommand("fontSize", false, size); // 5 - размер шрифта (зависит от браузера)
    },

    checkText() {
      // console.log(this.$refs.editable);
      const preview = this.$refs.editable.outerHTML.replace('contenteditable="true"', "");
      const resuling = preview.replace(/(?<=<(b|i))[^>]*\s+style="([^"]*)"[^>]*(?=>)/g, "");
      console.log(this.$refs.editable);
      this.result = resuling;
      // console.log(preview.outerHTML.replace('contenteditable="true"', ""));
      // this.result = preview.outerHTML.replace('contenteditable="true"', "");
    }
  },

  // watch: {
  //   inputText(newValue) {
  //     console.log(newValue);
  //   }
  // }

  mounted() {
    this.$refs.editable.addEventListener("paste", (e) => {
      e.preventDefault();
      const text = (e.originalEvent || e).clipboardData.getData("text/plain");
      document.execCommand("insertText", false, text);
    });
  }
};
</script>

<style scoped>
.editor-toolbar {
  padding: 10px;
}
.editor {
  width: 100%;
  height: 500px;
  border: 1px solid #ccc;
  padding: 10px;
  font-family: Arial, sans-serif;
  font-size: 16px;
  background-color: #f9f9f9;
}

.edit-area {
  outline: none;
  height: 100%;
}

.size {
  margin-left: 5px;
}
</style>
