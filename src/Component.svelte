<script>
  import { getContext } from "svelte";
  import { Transformer } from "markmap-lib";
  import * as markmap from "markmap-view";
  import { afterUpdate } from "svelte";
  import YAML from "hexo-front-matter";

  export let value = "# niv0\n\n## niv1\n- niv1.1\n- niv1.2";
  export let brancheWidth = 0;
  export let duration = 100;
  export let mindHeight = 500;
  export let spacingVertical = 8;
  export let spacingHorizontal = 50;
  export let paddingX = 5;
  export let color = "defaut";

  let mindWidth = "100%";
  let markdown;
  let mindmap;
  let expand = true;
  let opts = YAML.parse(value, { separator: "\n---\n" }) || {};

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  function createSVG(mm) {
    mm = mm.innerHTML;
    mm = mm.replace(/<br>/g, "<br/>");
    mm = mm.replace(/\n/g, " ");
    mm =
      '<svg id="markmap" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" class="' +
      mindmap.className["baseVal"] +
      '" style="width: 100%; height: ' + mindHeight + 'px;">' +
      mm +
      "</svg>";
    return mm;
  }

  function saveData() {
    let textToWrite = createSVG(mindmap);
    let textFileAsBlob = new Blob([textToWrite], { type: "text/plain" });
    let downloadLink = document.getElementById("jta");
    downloadLink.download = "mindmap.svg";
    //downloadLink.innerHTML = "Télécharger";
    downloadLink.href = window.URL.createObjectURL(textFileAsBlob);
    downloadLink.click();
    window.URL.revokeObjectURL(downloadLink.href);
  }

  function replaceMarkdown(md) {
    md = md.replace(
      /(?<!#)# (.*)\n/g,
      '# <span style="font-weight:bold; font-size:1.3em; display:block; padding-bottom:0.6em">$1</span>\n'
    );
    md = md.replace(
      /(?<!#)## (.*)\n/g,
      '## <span style="font-weight:bold; font-size:1em; display:block; padding-bottom:0.4em">$1</span>\n'
    );
    return md;
  }

  $: markdown = replaceMarkdown(
    YAML.parse(value, { separator: "\n---\n" })._content
  );

  function expandOuiNon() {
    expand = !expand;
  }

  $: expand = expand;

  afterUpdate(() => {
    const transformer = new Transformer();

    const { root, features } = transformer.transform(markdown);
    const { styles, scripts } = transformer.getUsedAssets(features);
    const { Markmap, loadCSS, loadJS } = markmap;

    if (styles) loadCSS(styles);
    if (scripts) loadJS(scripts, { getMarkmap: () => markmap });

    const options = {
      duration: duration,
      style: (id) => "div{padding-bottom:0.12em!important}",
      spacingVertical: spacingVertical,
      spacingHorizontal: spacingHorizontal,
      maxWidth: brancheWidth,
      paddingX: paddingX,
    };

    if (color !== "defaut")
      options.color = function (elt) {
        return color;
      };

    if (opts.markmap) {
      if (opts.markmap.colorFreezeLevel !== undefined)
        options.colorFreezeLevel = opts.markmap.colorFreezeLevel;
      if (opts.markmap.initialExpandLevel !== undefined)
        options.initialExpandLevel = opts.markmap.initialExpandLevel;
    }

    if (expand === false) options.initialExpandLevel = 0;

    mindmap.innerHTML = "";
    Markmap.create("#markmap", options, root);
  });
</script>

<div use:styleable={$component.styles}>
  <div id="menu">
    <a href="view" title="ferme/ouvre" on:click|preventDefault={expandOuiNon}>
      {#if expand}
      <i class="ri-arrow-left-right-fill ri-lg svelte-1ghy1wa"></i>
      {:else}
      <i class="ri-arrow-left-right-fill ri-lg svelte-1ghy1wa"></i>
      {/if}
    </a>
    <a href="#saveSVG" title="Enregistrer" on:click|preventDefault={saveData}>
      <i class="ri-save-line ri-lg svelte-1ghy1wa"></i>
    </a>
    <a href="#saveSVG" style="display:none" id="jta">clic</a>
  </div>
  <div id="mind">
    <svg
      id="markmap"
      bind:this={mindmap}
      style="width: {mindWidth}; height: {mindHeight}px"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
    />
  </div>
</div>

<style>
  #mind {
    display: flex;
  }
  #menu {
    margin: 5px;
  }
  #menu a {
    margin-left: 5px;
    text-decoration: none;
  }
</style>
