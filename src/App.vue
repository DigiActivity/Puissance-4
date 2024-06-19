<script setup>
import { ref, computed } from "vue";
// Notre grille fait 7 colonnes et 6 lignes
const grille = ref([
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
]);

const couleurActuelle = ref(1);

const computedJoueur = computed(() => {
  if (couleurActuelle.value === 1) {
    return { class: "yellow", couleur: "Jaune" };
  } else {
    return { class: "red", couleur: "Rouge" };
  }
});

function changeTour() {
  // échanger la couleur actuelle
  if (couleurActuelle.value === 1) {
    couleurActuelle.value = 2;
  } else {
    couleurActuelle.value = 1;
  }
}

function verifieVictoire(couleur, colonne, cellule) {
  // couleur : un 1 ou un 2 (jaune ou rouge)

  let compteur = 0;
  // Horizontal : vérifier qu'on a au moins 4 pièces alignées

  let x = colonne;
  let y = cellule;

  // on regarde à droite

  /**
   * Récapitulons les étapes de vérification :
   * - Est ce que j'ai déjà 4 pions alignés ?
   * - Est ce que je suis sorti du tableau ?
   * - Est ce que sur la case actuelle il y a ma couleur ?
   */
  while (compteur < 4 && x < 7 && grille.value[x][y] === couleur) {
    x++;
    compteur++;
  }
  if (compteur > 3) return true;

  // on se replace et on regarde à gauche
  x = colonne - 1;
  while (compteur < 4 && x >= 0 && grille.value[x][y] === couleur) {
    x--;
    compteur++;
  }
  if (compteur > 3) return true;

  // Vertical

  compteur = 0;

  x = colonne;
  y = cellule;

  while (compteur < 4 && y < 6 && grille.value[x][y] === couleur) {
    y++;
    compteur++;
  }
  if (compteur > 3) return true;

  y = cellule - 1;

  while (compteur < 4 && y >= 0 && grille.value[x][y] === couleur) {
    y--;
    compteur++;
  }
  if (compteur > 3) return true;

  // Diagonale droite

  compteur = 0;

  x = colonne;
  y = cellule;

  while (compteur < 4 && y < 6 && x < 7 && grille.value[x][y] === couleur) {
    x++;
    y++;
    compteur++;
  }
  if (compteur > 3) return true;

  x = colonne - 1;
  y = cellule - 1;

  while (compteur < 4 && y >= 0 && x >= 0 && grille.value[x][y] === couleur) {
    x--;
    y--;
    compteur++;
  }
  if (compteur > 3) return true;

  // Diagonale gauche

  compteur = 0;

  x = colonne;
  y = cellule;

  while (compteur < 4 && y < 6 && x >= 0 && grille.value[x][y] === couleur) {
    x--;
    y++;
    compteur++;
  }
  if (compteur > 3) return true;

  x = colonne + 1;
  y = cellule - 1;

  while (compteur < 4 && y >= 0 && x < 7 && grille.value[x][y] === couleur) {
    x++;
    y--;
    compteur++;
  }
  if (compteur > 3) return true;

  return false;
}

function onColumnClick(numero_colonne) {
  // utile en lecture, inutilisable en écriture
  const colonne = grille.value[numero_colonne];

  for (const [positionCellule, valeurCellule] of colonne.entries()) {
    if (valeurCellule === 0) {
      // alors je peux placer ma couleur à la position de la cellule actuelle
      grille.value[numero_colonne][positionCellule] = couleurActuelle.value;
      const isVictoire = verifieVictoire(
        couleurActuelle.value,
        numero_colonne,
        positionCellule
      );
      if (isVictoire) {
        alert("Victoire de : " + computedJoueur.value.couleur);
      }
      changeTour();
      break;
    }
  }
}
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
    <h2 :class="computedJoueur.class">
      C'est au tour du {{ computedJoueur.couleur }}
    </h2>
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

h2 {
  width: fit-content;
  margin: 30px auto;
}

h2.yellow {
  background-color: gold;
}
h2.red {
  background-color: red;
}
</style>
