<template>
  <editor-content :editor="editor" />
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-3'
import { defaultExtensions } from '@tiptap/starter-kit'
import Collaboration from '@tiptap/extension-collaboration'
import CollaborationCursor from '@tiptap/extension-collaboration-cursor'
import * as Y from 'yjs'
import { WebrtcProvider } from 'y-webrtc'
import { uniqueNamesGenerator, adjectives, colors, animals } from 'unique-names-generator';


export default {
  components: {
    EditorContent,
  },

  data() {
    return {
      editor: null,
      provider: null,
    }
  },

  mounted() {
    const ydoc = new Y.Doc()
    this.provider = new WebrtcProvider('tiptap-collaboration-cursor-extension', ydoc)
    const name = uniqueNamesGenerator({
      dictionaries: [adjectives, animals, colors],
      length: 3
    });
    const color = '#' + ('000000' + Math.floor(Math.random()*16777215).toString(16)).slice(-6);

    this.editor = new Editor({
      autofocus: true,
      extensions: [
        ...defaultExtensions(),
        Collaboration.configure({
          document: ydoc,
        }),
        CollaborationCursor.configure({
          provider: this.provider,
          user: {
            name,
            color,
          },
        }),
      ],
    })
  },

  beforeUnmount() {
    this.editor.destroy()
    this.provider.destroy()
  },
}
</script>

<style>
/* Give a remote user a caret */
.collaboration-cursor__caret {
  position: relative;
  margin-left: -1px;
  margin-right: -1px;
  border-left: 1px solid #0D0D0D;
  border-right: 1px solid #0D0D0D;
  word-break: normal;
  pointer-events: none;
}

/* Render the username above the caret */
.collaboration-cursor__label {
  position: absolute;
  top: -1.4em;
  left: -1px;
  font-size: 12px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  user-select: none;
  color: #0D0D0D;
  padding: 0.1rem 0.3rem;
  border-radius: 3px 3px 3px 0;
  white-space: nowrap;
}

blockquote {
  border-left: 1px solid gray;
  padding-left: 10px;
}
</style>