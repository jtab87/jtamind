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

  export let clickItem;

  function clicx1(id) {
    clickItem({
      value: "toto=" + id,
    });
  }

  let mindWidth = "100%";
  let markdown;
  let mindmap;
  let expand = true;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  /*
  $: {
    console.log(YAML.parse(value, { separator: "\n---\n" }));
  }
*/
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
      //style: id => 'div{padding-bottom:0.25em!important} g g:last-of-type div{font-weight:bold; font-size:18px} foreignObject{overflow:visible!important; transform:translateX(-1%)} g g:last-of-type rect {transform:scaleX(125%) translateX(-3%)}',
      //style: id => 'div{padding-bottom:0.3em!important} g g:last-of-type div{font-weight:bold;} foreignObject{overflow:visible!important; transform:translateX(-1%)}',
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
    if (expand === false) options.initialExpandLevel = 0;

    mindmap.innerHTML = "";
    Markmap.create("#markmap", options, root);
  });
</script>

<div use:styleable={$component.styles}>
  <div id="menu">
    <a href="view" title="ferme/ouvre" on:click|preventDefault={expandOuiNon}>
      {#if expand}⬅️{:else}➡️{/if}
    </a>
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
