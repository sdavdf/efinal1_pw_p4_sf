<template>
  <div class="juego">
    <h1 class="title">Juego Casino-Pokemon</h1>

    <div class="cuadros-container">
      <div v-if="!juegoTerminado" v-for="(pokemon, index) in pokemones" :key="pokemon.id" class="pokemon">
        <div v-if="!pokemon.image" class="cuadro-negro"></div>
        <img v-else :src="pokemon.image" :alt="pokemon.name" />
        <p v-if="!pokemon.image">xxxxxxxxxxx</p>
        <p v-if="pokemon.image">{{ pokemon.name }}</p>
      </div>
    </div>

    <button v-if="!juegoTerminado" @click="jugar" class="jugar-button">JUGAR</button>

    <div v-if="!juegoTerminado" class="results-container">
      <div class="result">
        <h3>Intentos: {{ intentos }}</h3>
      </div>
      <div class="result">
        <h3>Puntos: {{ puntos }}</h3>
      </div>
    </div>

    <div v-if="juegoTerminado" class="game-gana">
    
      <p v-if="hasGanado">
        <dir>Puntaje: {{ puntos }}</dir>
        Felicitaciones has ganado un premio de $10.000,00
      </p>
      <p v-else class="game-termina">
       <dir>Has utilizado tus 5 intentos</dir>

        El juego ha terminado, intentalo nuevamente
      </p>
      <button @click="reiniciarJuego" class="reiniciar-button">Nuevo Juego</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pokemones: [
        { id: 1, name: '', image: null },
        { id: 2, name: '', image: null },
        { id: 3, name: '', image: null }
      ],
      puntos: 0,
      intentos: 0,
      juegoTerminado: false
    };
  },

  computed: {
    hasGanado() {
      return this.puntos >= 10 && this.intentos <= 5;
    }
  },

  methods: {
    async jugar() {
      if (this.juegoTerminado) return; 

      const pokemonIds = [9, 22, 33, 44, 55]; // IDs de los Pokémon a mostrar 

      try {
        // Generar los IDs aleatorios de los Pokémon
        const randomIds = [];
        for (let i = 0; i < this.pokemones.length; i++) {
          const randomIndex = Math.floor(Math.random() * pokemonIds.length);
          const randomId = pokemonIds[randomIndex];
          randomIds.push(randomId);
        }

        // Obtener los datos de los Pokémon
        for (let i = 0; i < randomIds.length; i++) {
          const id = randomIds[i];
          const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
          const data = await response.json();

          this.pokemones[i].name = data.name;
          this.pokemones[i].image = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${id}.svg`;
        }

        // Calcular los puntajes e intentos
        const idsSet = new Set(randomIds);
        if (idsSet.size === 1) {
          // Si coinciden 3 imagenes
          this.puntos += 5;
        } else if (idsSet.size === 2) {
          // Si coinciden 2 imagenes
          this.puntos += 2;
        }else if (idsSet.size === 0) {
          // Si no coinciden ninguna imagen 
          this.puntos += 0;
        }
        this.intentos++;

        if (this.intentos > 5 || this.puntos >= 10) {
          this.juegoTerminado = true; // Marcar el juego como terminado si se superan los 5 intentos o se alcanzan los 10 puntos
        }
      } catch (error) {
        console.error(error);
    
      }
    },

    reiniciarJuego() {
      this.puntos = 0;
      this.intentos = 0;
      this.juegoTerminado = false;
      this.pokemones.forEach(pokemon => {
        pokemon.name = '';
        pokemon.image = null;
      });
    }
  }
};
</script>

<style scoped>
.juego {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
}

.cuadros-container {
  display: flex;
}

.pokemon {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 10px;
}

.pokemon img {
  width: 200px;
  height: 200px;
  margin-bottom: 10px;
}

.cuadro-negro {
  width: 200px;
  height: 200px;
  background-color: black;
}
.jugar-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #3498db;
  color: #fff;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.results-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.result {
  margin: 0 10px;
}

.result h3 {
  font-size: 18px;
  font-weight: bold;
}

.reiniciar-button {
  margin-top: 10px;
  padding: 8px 16px;
  background-color: #dded85;
  color: white;
  font-size: 14px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.reiniciar-button:hover {
background-color: aquamarine;  
}


.game-termina {
  margin-top: 20px;
  font-weight: bold;
  color: rgb(255, 0, 0);
}

.game-gana {
  margin-top: 20px;
  font-weight: bold;
  color: blue;
}


</style>
