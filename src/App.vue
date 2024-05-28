<script setup>
import { ref } from "vue";
// Notre grille fait 7 colonnes et 6 lignes
const grille = ref([
  [0, 0, 0, 0, 0, 0],
  [1, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
  [1, 2, 0, 0, 0, 0],
  [1, 0, 0, 0, 0, 0],
  [2, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0],
]);

const couleurActuelle = ref(1);

function onColumnClick(numero_colonne) {
  // parcourir la colonne 'numero_colonne'
  // dès que je trouve une cellule vide, je place ma couleur
  // si j'ai placé, le tour est validé et la couleur change
  // ------------------------------------------------------------
  // si je n'ai pas réussi, c'est que la colonne est pleine
  // le tour n'est pas validé, c'est toujours à moi
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
</style>
