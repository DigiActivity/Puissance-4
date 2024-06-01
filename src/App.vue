<script setup>
import { ref } from "vue";
// Notre grille fait 7 colonnes et 6 lignes
const grille = ref([
  [0, 0, 0, 0, 0, 0],
  [1, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [1, 2, 1, 2, 1, 2],
  [1, 0, 0, 0, 0, 0],
  [2, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
]);

let couleurActuelle = ref(1);
let TextJoueur = ref(("Joueur 1 avec la couleur rouge joue"));


function onColumnClick(numero_colonne) {
  // parcourir la colonne 'numero_colonne'
  // dès que je trouve une cellule vide, je place ma couleur
  // si j'ai placé, le tour est validé et la couleur change
  // ------------------------------------------------------------
  // si je n'ai pas réussi, c'est que la colonne est pleine
  // le tour n'est pas validé, c'est toujours à moi

  // couleur 1 = rouge
  // couleur 2 = jaune
  
  for ( let y = 0; y < grille.value[numero_colonne].length; y++) {
    

    if (grille.value[numero_colonne][y] === 0) {
      
      console.log(grille)
      console.log(couleurActuelle.value)
      grille.value[numero_colonne][y] = couleurActuelle.value;

      couleurActuelle.value = couleurActuelle.value === 1 ? 2 : 1;
      TextJoueur.value = couleurActuelle.value === 1 ? "Joueur 1 avec la couleur rouge joue" : "Joueur 2 avec la couleur jaune joue";

      

      // si condition valide ? vrai : false
      
      break;
    }
  }

}
// grille.value[numero_colonne][y] = couleurActuelle.value;
// // couleurActuelle.value = couleurActuelle.value === 1 ? 2 : 1;
</script>

<template>
  <h1>Puissance 4</h1>
  <main>

    <div class="grille">
      <div
        class="colonne"
        v-for="(colonne, x) in grille"
        :key="x"
        @click="onColumnClick(x)"
      >
        <div
          class="cellule"
          :class="{ jaune: cellule === 1, rouge: cellule === 2 }"
          v-for="(cellule, y) in colonne"
          :key="y"
        ></div>
      </div>
    </div>
    <h2
    :class="couleurActuelle === 1 ? 'texteRouge' : 'TexteJaune'"
    
    >{{ TextJoueur }}</h2>
  </main>
</template>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
h1 {
  text-align: center;
}
.grille {
  background-color: rgb(34, 34, 120);
  display: flex;
  width: fit-content;
  margin: 0 auto;
  padding: 15px;
}

.colonne {
  display: flex;
  flex-direction: column-reverse;
}

.colonne:hover {
  background-color: rgb(65, 65, 160);
  border-radius: 100px;
  cursor: pointer;
}

.cellule {
  width: 60px;
  height: 60px;
  background-color: white;
  border-radius: 100%;
  margin: 5px;
}

.cellule.rouge {
  background-color: rgb(223, 64, 29);
}

.cellule.jaune {
  background-color: rgb(234, 196, 27);
}
.TexteJaune {
  color: rgba(209, 171, 0, 0.874);
}
.texteRouge {
  color: rgb(124, 32, 11);
}
</style>
