# Jtamind

# Description
Plugin MindMap pour budiBase

Permet de transformer un contenu au format **MarkDown** en MindMap

## configuration

- **Contenu** : Le contenu au format markdown (Voir exemple ci dessous)
- **Hauteur** : hauteur de la fenetre qui contiendra le mindmap (minimum : 200px)
- **Tempo**. : la temporisation (en ms) utilisée pour faire apparaitre / disparaitre les branches. (0 = pas de temporisation)
- **Espacement vertical** : espace vertical entre les branches 
- **Espacement horizontal** : espace horizontal entre les noeuds d'une même branche
- **Padding X** : espace entre le noeud et le texte
- **Largeur max** : largeur maximum d'une branche (0 = pas de maximum)
- **Couleur** : Couleur des branches ("Défaut" = couleurs par défaut)

> **Attention** : Ce composant masquera tous les composants placés **en dessous**. 

## Manipulation du MapMind

- Molette souris : Zoom +/-
- ⬅️➡️ : ouvrir / fermer tous les noeuds
- Cliquer + tirer : déplace le mindmap

## Exemple de markdown

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
- avec un emoj 🔶
```
# Code ajeter
```
let rows = $("DP ofn.Rows");
let niveau = 0;
let ret = ["# OFN2"];

function encadre(libelle, nature, pere) {
  if(nature == "A") return "- ==" + libelle + "==";
  if(pere == "root") return "## **" + libelle + "**";
  return "### **" + libelle + "**";
}

function cherchefils(pere) {
  for(let i=0; i<rows.length; i++) {
    if(rows[i].pere == pere) {
      // Je suis le fils de "pere"
      ret.push(encadre(rows[i].libelle, rows[i].nature, pere));

      // je cherche mes fils
      niveau++;
      if(rows[i].nature == "P") cherchefils(rows[i].code);
      niveau--;
    }
  }
}
cherchefils("root");
return ret.join("\n");
```

# Librairies utilisées

- Utilise les composants **markmap-lib** et **markmap-view** de la librairie [markmap](https://markmap.js.org/docs/markmap) 
- Inspiré par [cet exemple](https://svelte.dev/repl/9499dbcf3f3240e4af42e38ab19cc9ea?version=3.47.0)

Find out more about [Budibase](https://github.com/Budibase/budibase).
