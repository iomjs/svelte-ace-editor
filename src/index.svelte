<div
  bind:this={element}
  style="width:{width ? px(width) : '100%'};height:{height ? px(height) : '100%'}"
></div>

<!--
	allow access to props like
	<AceEditor bind:this={editorComponent} />
	let editorComponent;
	editorComponent.editor.resize();
-->
<svelte:options accessors />

<script>
  import { createEventDispatcher, tick, onMount, onDestroy } from 'svelte';

	import * as ace from 'brace';
	import 'brace/ext/emmet';

  const dispatch = createEventDispatcher();
  /**
   * translation of vue component to svelte:
   * @link https://github.com/chairuosen/vue2-ace-editor/blob/91051422b36482eaf94271f1a263afa4b998f099/index.js
   **/
  export let value; // String, required
  export let lang; // String
  export let theme; // String
  export let height = null; // null for 100, else integer, used as percent
  export let width = null; // null for 100, else integer, used as percent
  export let options = null; // Object

  let element = null; // bind this element to variable
  export let editor = null;
  let contentBackup = '';

  onDestroy(() => {
    if (editor) {
      editor.destroy();
      editor.container.remove();
    }
  });

  $: watchValue(value);
  function watchValue (val) {
    if (contentBackup !== val && editor && typeof val === 'string') {
      editor.session.setValue(val,1);
      contentBackup = val;
    }
  }

  $: watchTheme(theme);
  function watchTheme (newTheme) {
    if (editor) {
      editor.setTheme('ace/theme/'+newTheme);
    }
  }

  $: watchMode(lang);
  function watchMode (newOption) {
    if (editor) {
      editor.getSession().setMode('ace/mode/'+lang);
    }
  }

  $: watchOptions(options);
  function watchOptions (newOption) {
    if (editor) {
      editor.setOptions(newOption);
    }
  }

  export const resizeOnNextTick = () => tick().then(() => {
    if (editor) {
      editor.resize();
    }
  });

  $: if (height!==null && width!==null) { resizeOnNextTick(); }

  if (true) {
    onMount(() => {
      lang = lang || 'text';
      theme = theme || 'chrome';

      editor = ace.edit(element);

      dispatch('init', editor);

      editor.$blockScrolling = Infinity;
      //editor.setOption("enableEmmet", true);
      editor.getSession().setMode('ace/mode/'+lang);
      editor.setTheme('ace/theme/'+theme);
      editor.setValue(value, 1);
      contentBackup = value;

      editor.on('change',function () {
        const content = editor.getValue();
        dispatch('input', content);
        contentBackup = content;
      });
      if (options) {
        editor.setOptions(options);
      }
    });
  }

  const ValidPxDigitsRegEx = /^\d*$/;
  function px (n) {
    if (ValidPxDigitsRegEx.test(n) ){
        return n+"px";
    }
    return n;
  }
</script>
