svelte-ace-editor
====================


[![npm](https://img.shields.io/npm/v/svelte-ace-editor.svg)](https://www.npmjs.com/package/svelte-ace-editor)


A packaging of [ace](https://ace.c9.io/)

Demos:
- https://sql.greatenemy.com/ https://github.com/greatenemy/sql-sapper
- https://textsort.greatenemy.com/ https://github.com/greatenemy/textsort-sapper

## IMPORTANT
emmet support for html is removed after 0.0.6. because its code cannot works with strict mode.

if you want to use it. require emmet by your own.
```
npm install emmet@git+https://github.com/cloud9ide/emmet-core.git#41973fcc70392864c7a469cf5dcd875b88b93d4a
```

```js
require(['emmet/emmet'],function (data) { // this is huge. so require it async is better
    window.emmet = data.emmet;
});
```

## How to use

1. Install

    ```
    npm install --save-dev svelte-ace-editor
    ```

2. Use in a svelte component:

    ```js
    <div>
        <AceEditor
          bind:value={value}
          theme="clouds_midnight"
          lang={lang}
          options={options}
          width="100%"
          height="512"
          on:init={editorInit}
          on:input={onEditorChange}
        />
    </div>

    <script>
      import AceEditor from 'svelte-ace-editor'

      if (process.browser) {
        require('brace');
        // require('brace/ext/language_tools')
        // require('brace/ext/language_tools')
        // require('brace/mode/pgsql')
        // require('brace/mode/mysql')
        // require('brace/mode/sql')
        // require('brace/mode/sqlserver')
        require('brace/theme/clouds_midnight')
      }

      let value = ''
      let lang = 'sql'
      let options = {}

      function editorInit (editor) {
        // @todo something with editor or something after init
      }
      function onEditorChange (newValue) {
        value = newValue.detail;
      }

    </script>
    ```

3. Further Notes:

    prop `value`  is required

    prop `lang` and `theme` is same as [ace-editor's doc](https://github.com/ajaxorg/ace)

    prop `height` and `width` could be one of these:  `200`, `200px`, `50%`

    this component is based on this component: https://github.com/chairuosen/vue2-ace-editor/tree/91051422b36482eaf94271f1a263afa4b998f099
