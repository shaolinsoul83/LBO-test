<template>
  <div class="app">
    <h1>Calcul du plus grand produit des sous-chaînes</h1>

    <!-- Formulaire -->
    <form @submit.prevent="calculateMaxProduct">
      <label>
        Chaîne de nombres :
        <input v-model="numberString" type="text" placeholder="Entrez une chaîne de nombres" required />
      </label>
      <label>
        Longueur n :
        <input v-model.number="n" type="number" min="1" placeholder="Entrez la valeur de n" required />
      </label>
      <button type="submit">Calculer</button>
    </form>

    <!-- Résultats -->
    <div v-if="result">
      <h2>Résultats :</h2>
      <p>
        <strong>Plus grand produit :</strong> {{ result.maxProduct }}
      </p>
      <p>
        <strong>Sous-chaîne :</strong> "{{ result.maxSequence }}"
      </p>
    </div>

    <!-- Exemples -->
    <div class="examples">
      <h2>Exemples :</h2>
      <div v-for="(example, index) in examplesWithResults" :key="index" class="example">
        <p>
          <strong>Exemple {{ index + 1 }} :</strong><br />
          Chaîne : {{ example.numberString }}<br />
          Longueur n : {{ example.n }}<br />
          Plus grand produit : {{ example.result.maxProduct }}<br />
          Sous-chaîne : "{{ example.result.maxSequence }}"
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

// refs
const numberString = ref("");
const n = ref(0);
const result = ref(null);

// Exemples
const examples = [
  { numberString: "1127839561", n: 5 },
  { numberString: "1127839561", n: 3 },
  { numberString: "12345654355", n: 4 },
];

// Calcul du produit maximal
const calculateMaxProductTool = (numberString, n) => {
  // Vérifier si numberString contient uniquement des chiffres
  if (!/^[0-9]+$/.test(numberString)) {
    return { maxProduct: 0, maxSequence: "Erreur : La chaîne contient des caractères non numériques" };
  }

  // Vérifier si n est bien valide
  if (n > numberString.length || n <= 0) {
    return { maxProduct: 0, maxSequence: "Erreur : Longueur invalide" };
  }

  if (n > numberString.length || n <= 0) {
    return { maxProduct: 0, maxSequence: "" };
  }

  let maxProduct = 0;
  let maxSequence = "";

  for (let i = 0; i <= numberString.length - n; i++) {
    const substring = numberString.slice(i, i + n);
    let product = 1;

    for (const char of substring) {
      product *= parseInt(char, 10);
    }

    if (product > maxProduct) {
      maxProduct = product;
      maxSequence = substring;
    }
  }

  return { maxProduct, maxSequence };
};

// Calcul résultat
const calculateMaxProduct = () => {
  result.value = calculateMaxProductTool(numberString.value, n.value);
};

// Calcul des exemples
const examplesWithResults = examples.map((example) => {
  const result = calculateMaxProductTool(example.numberString, example.n);
  return { ...example, result };
});
// const examplesWithResults = computed(() =>
//   examples.map((example) => ({
//     ...example,
//     result: calculateMaxProductTool(example.numberString, example.n),
//   }))
// );
</script>

<style>
.app {
  font-family: 'Roboto', Arial, sans-serif;
  max-width: 700px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 12px;
  background: linear-gradient(135deg, #f8f9fa, #e9ecef);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  color: #343a40;
}

form {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 30px;
}

form label {
  font-size: 1rem;
  font-weight: 500;
  color: #495057;
}

form input {
  padding: 12px 15px;
  font-size: 1rem;
  border: 1px solid #ced4da;
  border-radius: 8px;
  background-color: #fff;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

form input:focus {
  border-color: #42b983;
  box-shadow: 0 0 5px rgba(66, 185, 131, 0.5);
  outline: none;
}

button {
  padding: 12px 15px;
  font-size: 1rem;
  font-weight: bold;
  color: white;
  background: linear-gradient(135deg, #42b983, #2c8e67);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease, transform 0.2s ease;
}

button:hover {
  background: linear-gradient(135deg, #2c8e67, #216b4c);
  transform: translateY(-2px);
}

button:active {
  transform: translateY(0);
}

h1,
h2 {
  color: #212529;
  margin-bottom: 20px;
}

.examples {
  margin-top: 30px;
  border-top: 2px solid #dee2e6;
  padding-top: 20px;
}

.example {
  margin-bottom: 20px;
  padding: 15px;
  border-radius: 8px;
  background-color: #f8f9fa;
  border: 1px solid #ced4da;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.example p {
  margin: 5px 0;
  color: #495057;
  line-height: 1.5;
}

.example strong {
  color: #42b983;
}
</style>