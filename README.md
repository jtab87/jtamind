# Jtamind

# Description
MindMap Plugin for budiBase

Allows transforming content in **MarkDown** format into a MindMap.

## configuration

- **Contenu**: Content in markdown format (See example below)
- **Hauteur**: Height of the window that will contain the mind map (minimum: 200px)
- **Tempo**: Delay (in ms) used to make branches appear/disappear. (0 = no delay)
- **Espacement vertical**: Vertical space between branches
- **Espacement horizontal**: Horizontal space between nodes in the same branch
- **Padding X**: Space between node and text
- **Largeur max**: Maximum width of a branch (0 = no maximum)
- **Couleur**: Branches' color ("Defaut" = default colors)
- **Clic sur item**: event executed when clicking on a label 

> **Warning**: This component will hide all components placed **below**

You can add an identifier for each item, by pasting it at the end, **separated by an "@"**. Examples: 
```
## section@identifiant

- libellé@identifiant
```
This identifier will be **hidden on display**, but will be offered in the "Click sur item" event

<img src="./src/images/2.png" width="200px" height="200px"/>


### An additional header can be added in the markdown
 
```
---
markmap:
  key: value
  key: value
---
```
This header allows setting the display parameters mentioned [HERE](https://markmap.js.org/docs/json-options). Some examples: 
- colorFreezeLevel: 0
- initialExpandLevel: 1
- ...

## Manipulation of MapMind

- **Mouse wheel**: Zoom +/-
- ⬆️➡️ : Expand / collapse all nodes
-  💾 : Save the SVG
- **Click + drag**: Moves the mind map
- **Click on label** : execute the "Click sur item" event

## Example

### The markdown
```
# racine

## Liens

- <https://markmap.js.org/>
- [GitHub](https://github.com/gera2ld/markmap)

## Quelques mises en forme

- **strong** ~~del~~ *italic* ==highlight==
- multiline
  text
- `inline code`
- Avec un emoji 🔶
```
### The result
<img src="./src/images/1.png" width="600px" height="400px"/>

# Used Libraries

- Utilizes **markmap-lib** and **markmap-view** components from the library [markmap](https://markmap.js.org/docs/markmap) 
- Inspired by [this example](https://svelte.dev/repl/9499dbcf3f3240e4af42e38ab19cc9ea?version=3.47.0)

<hr>

Find out more about [Budibase](https://github.com/Budibase/budibase).

