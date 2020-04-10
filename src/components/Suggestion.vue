<template>
  <div>
      <span style="font-size:40px;display:block;margin-bottom:20px;color:#000000"><i class="el-icon-document"></i><b>请留下您宝贵的意见</b><i class="el-icon-lollipop"></i></span>
    <quill-editor
      ref="myTextEditor"
      v-model="content"
      :config="editorOption"
      @blur="onEditorBlur($event)"
      @focus="onEditorFocus($event)"
      @ready="onEditorReady($event)"
      style="height:480px;"
    ></quill-editor>
    
    <el-button type="primary" style="margin-top:50px;float:right" @click="submit">提交</el-button>
  </div>
</template>

<script>
import { quillEditor } from "vue-quill-editor";

import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
// use
export default {
  components: {
    quillEditor
  },
  data() {
    return {
      content: "<h2>I am Example</h2>",
      editorOption: {
        // something config
      }
    };
  },
  methods: {
    onEditorBlur(editor) {
      console.log("editor blur!", editor);
    },
    onEditorFocus(editor) {
      console.log("editor focus!", editor);
    },
    onEditorReady(editor) {
      console.log("editor ready!", editor);
    },
    onEditorChange({ editor, html, text }) {
      // console.log('editor change!', editor, html, text)
      this.content = html;
    },
    submit(){
        console.log("提交提交提交")
    }
  }
};
</script>