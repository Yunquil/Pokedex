<template>
  <div class="jose">
    <div class="contenedor1">
      <div class="input-container">
        <input
          type="text"
          placeholder="Busca tu Pokemon"
          name="text"
          class="input"
        />
        <button class="button1" @click="buscarPokemon"></button>
      </div>
    </div>
<div class="filtro">
  <h1 class="titulo1">POKEMONES</h1>
  <select v-model="filtroTipo" class="custom-select">
          <option value="">Todos los tipos</option>
          <option v-for="(tipo, index) in tiposPokemon" :key="index" :value="tipo">
            {{ tipo }}
          </option>
        </select>
</div>
    
    <div class="contenedor2">
      <div
        class="tarjet"
        v-for="(pokemon, index)  in filtrarPokemon()"
        :key="index"
        @click="mostrarModal(pokemon)"
        :style="{ backgroundColor: getColorByType(pokemon.tipo_pk[0]) }"
      >
        <p class="infor2">{{ pokemon.nombre }}</p>
        <img class="imagen-pokemon" :src="pokemon.img" alt="" />
        <p class="infor1">ID:#{{ pokemon.numero }}</p>
        <div class="info2">
          <p v-for="(tipo, index) in pokemon.tipo_pk" :key="index">
            {{ tipo }}
          </p>
        </div>
      </div>
      <button class="button2" @click="cargarMasPokemones(50)">Cargar más...</button>
    </div>

    <div v-if="info_modal" class="modal">
      <div class="modal-content">
        <div class="boton">
          <button @click="ocultarModal" class="btn2">❌</button>
        </div>

        <div class="container1">
          <div class="nombre">
            <header class="header2" :style="{ backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"></header>
            <h1 class="titulo2">{{ selectedPokemon.nombre }}</h1>
          </div>

          <div class="container overflow-hidden text-center">
            <div class="row gx-0">
              <div class="col">
                <div class="p-5">
                  <div class="pg-2">
                    <div class="primera-p">
                      <label for="">ID: </label>
                      <p class="info">#{{ selectedPokemon.numero }}</p>
                    </div>

                    <div class="primera-p">
                      <label for="">Height: </label>
                      <p class="info">{{ selectedPokemon.altura }}</p>
                    </div>

                    <div class="primera-p">
                      <label for="">Weight: </label>
                      <p class="info">{{ selectedPokemon.peso }}</p>
                    </div>

                    <div class="primera-p">
                      <label for="">Type: </label>
                      <p>{{ selectedPokemon.tipo_pk.join(", ") }}</p>
                    </div>
                  </div>
                </div>
              </div>

              <div class="col">
                <div class="p-5">
                  <img :src="selectedPokemon.img" alt="" class="imagenPK" />
                </div>
              </div>

              <div class="col">
                <div class="p-5">
                  <div class="pg-3">
                    <div class="contt">
                      <label for="HP">HP</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.hp"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.hp + '%',backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"
                        >
                          {{ selectedPokemon.hp }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Attack">Attack</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.ataque"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.ataque + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"
                        >
                          {{ selectedPokemon.ataque }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Defence">Defence</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.defensa"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.defensa + '%',backgroundColor: getColorByType(selectedPokemon.tipo_pk[0])}"
                        >
                          {{ selectedPokemon.defensa }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Sp. Attack">Sp. Attack</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.ataque_especial"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{
                            width: selectedPokemon.ataque_especial + '%',backgroundColor: getColorByType(selectedPokemon.tipo_pk[0])}"
                        >
                          {{ selectedPokemon.ataque_especial }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Sp. Defence">Sp. Defence</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.defensa_especial"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{
                            width: selectedPokemon.defensa_especial + '%',backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"
                          
                        >
                          {{ selectedPokemon.defensa_especial }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Speed">Speed</label>
                      <div
                        class="progress"
                        role="progressbar"
                        aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.velocidad"
                        aria-valuemin="0"
                        aria-valuemax="100"
                      >
                        <div
                          class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.velocidad + '%',backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"
                        >
                          {{ selectedPokemon.velocidad }}%
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, computed } from "vue";

let pokemones = ref([]);
let info_modal = ref(false);
let selectedPokemon = ref(null);
let filtroTipo = ref(''); 

async function obtenerUrlpokemon() {
  pokemones.value = [];

  for (let i = 1; i <= 50; i++) {
    let r = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}/`);
    let pokemon = {
      img: r.data.sprites.other["official-artwork"].front_default,
      numero: r.data.id,
      nombre: r.data.name,
      altura: r.data.height,
      peso: r.data.weight,
      tipo_pk: r.data.types.map((pk) => pk.type.name),
      hp: r.data.stats[0].base_stat,
      ataque: r.data.stats[1].base_stat,
      defensa: r.data.stats[2].base_stat,
      ataque_especial: r.data.stats[3].base_stat,
      defensa_especial: r.data.stats[4].base_stat,
      velocidad: r.data.stats[5].base_stat,
    };
    pokemones.value.push(pokemon);
  }
}



function getColorByType(tipo) {
  if (tipo === "fire") {
    return "#ff8a80";
  } else if (tipo === "grass") {
    return "#67e696";
  } else if (tipo === "electric") {
    return "#f7f72a";
  } else if (tipo === "water") {
    return "#81d4fa";
  } else if (tipo === "ground") {
    return "#f4e7da";
  } else if (tipo === "rock") {
    return "#544a28";
  } else if (tipo === "fairy") {
    return "#f8bbd0";
  } else if (tipo === "poison") {
    return "#ad8ee7";
  } else if (tipo === "bug") {
    return "#b5f57d";
  } else if (tipo === "dragon") {
    return "#37cc73";
  } else if (tipo === "psychic") {
    return "#eaeda1";
  } else if (tipo === "flying") {
    return "#c0daea";
  } else if (tipo === "fighting") {
    return "#de664e";
  } else if (tipo === "normal") {
    return "#bcaaa4";
  } else {
    return "white";
  }
}

async function cargarMasPokemones(cantidad) {
  const startIndex = pokemones.value.length + 1;
  const endIndex = startIndex + cantidad - 1;

  for (let i = startIndex; i <= endIndex; i++) {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`);
    const data = response.data;

    pokemones.value.push({
      img: data.sprites.other["official-artwork"].front_default,
      numero: data.id,
      nombre: data.name,
      hp: data.stats[0].base_stat,
      attack: data.stats[1].base_stat,
      defense: data.stats[2].base_stat,
      specialattack: data.stats[3].base_stat,
      specialdefense: data.stats[4].base_stat,
      speed: data.stats[5].base_stat,
      tipo_pk: data.types.map((element) => element.type.name),
    });
  }
}

function mostrarModal(pokemon) {
  selectedPokemon.value = pokemon;
  info_modal.value = true;
}

function ocultarModal() {
  info_modal.value = false;
}

obtenerUrlpokemon();

async function buscarPokemon() {
  const nombrePokemon = document.querySelector(".input").value.toLowerCase();
  const pokemonEncontrado = pokemones.value.find(
    (pokemon) => pokemon.nombre.toLowerCase() === nombrePokemon
  );

  if (pokemonEncontrado) {
    mostrarModal(pokemonEncontrado);
  } else {
    alert("Pokemon no encontrado");
  }
}
let tiposPokemon = computed(() => {
  const tipos = new Set();
  pokemones.value.forEach((pokemon) => {
    pokemon.tipo_pk.forEach((tipo) => {
      tipos.add(tipo);
    });
  });
  return Array.from(tipos);
});

function filtrarPokemon() {
  if (filtroTipo.value === '') {
    return pokemones.value;
  } else {
    return pokemones.value.filter((pokemon) =>
      pokemon.tipo_pk.includes(filtroTipo.value)
    );
  }
}

</script>

<style scoped>
.contenedor1 {
  background-image: url("assets/Pokemon-the-Movie-Volcanion-and-the-Mechanical_3840x2160.jpg");
  background-size: contain;
  object-fit: cover;
  height: 60vh;
  background-position: center;
  background-color: rgb(255, 204, 0);
}
.header1 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 56px;
  display: flex;
  align-items: center;
  background-color: #00000059;
}

.filtro{
  display: flex;
  flex-direction: row;
  gap: 100px;
  align-items: center;
}

.custom-select{
  background:linear-gradient(50deg,rgb(68, 95, 140),#ff5733) ;
  border-radius: 5px;
  height: 50px;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}
.titulo1 {
  font-family: "Comic Sans MS", cursive;
  font-size: 50px;
  color: #000000;
  text-shadow: 2px 2px 4px #000000;
  background-color: rgb(255, 204, 0);
  border: 2px solid #000000;
  padding: 10px;
  margin: 10px 0;
  width: 50%;
  margin-left: 25%;
}

.contenedor2 {
  display: grid;
  background-image: url("assets/pokemon2.png");
  background-repeat: no-repeat;
  background-position: center;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  text-align: center;
  width: 99%;
  margin-left: 5px;
  gap: 10px;
}
.tarjet {
  cursor: pointer;
  background-color: rgb(103, 230, 150);
    display: flex;
    flex-direction: column;
    align-items: center;
}

select {
  background-color: transparent;
  border: 0;
}

.contenedor2 > div {
  border: 1px solid #292727;
  padding: 20px;
  margin: 1px;
}

.titulo1 {
  text-align: center;
}

.input-container {
  width: 35%;
  position: absolute;
  left: 50%;
  top: 45%;
  transform: translate(-50%, -50%);
}

.imagen-pokemon {
  width: 150px;
  height: 150px;
}

.info2 {
  gap: 15px;
}

.infor2{
  background:white;
  border-radius: 10px ;
  width: 50%;
  height: 20px;
}
.modal {
  position: fixed;
  display: flex;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
  z-index: 9999;
  background-image: url(assets/wallpaperflare.com_wallpaper.jpg);
  color: white;
}

.modal-content {
  max-width: 80%; 
  width: auto;
  max-height: 90vh; 
  overflow-y: auto; 
  border-radius: 15px; 
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.4);
  position: relative;
  padding: 20px; 
  background-color: black;
  background-size: 100%;
  background-position: center;
}

.close-button {
  cursor: pointer;
  color: #ff0000; 
}

.modal-content h2 {
  color: #ff5733; 
  font-size: 28px;
  margin-bottom: 10px;
}

.modal-content p {
  color: #ffffff;
  font-size: 20px;
  line-height: 1.5;
}
.p-5 {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.container1 {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.col {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.button2{
  padding: 10px;
  margin: 10px;
  background:linear-gradient(50deg,rgb(68, 95, 140),#ff5733) ;
  border-radius: 5px;
  display: inline-block;
  width: 200px;
  text-align: center;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.boton {
  display: flex;
  justify-content: flex-end;
}
.btn2 {
  background-color: transparent;
  border: 0;
  cursor: pointer;
}

/* informacion de pokemon */
.header2 {
  width: 1200px;
  display: flex;
  height: 80px;
  border-radius: 10px 10px 0px 0px;
  background-color: rgb(84, 85, 85);
}

.nombre {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  position: absolute;
  width: 100%;
}
.titulo2 {
  position: relative;
  left: -21px;
  bottom: 64px;
}
.pg-2 {
  margin-top: 30%;
  perspective: 300px;
  transition: 0.3s;
}

.pg-3 {
  position: relative;
  top: 25px;
  width: 100%;
  perspective: 300px;
  transition: 0.3s;
}

.primera-p {
  display: flex;
  justify-content: space-between;
  margin: 25px;
  gap: 20px;
}

label {
  font-weight: 700;
  color: #ffffff;
  font-size: 20px;
}

.imagenPK {
  width: 200%;
  background-size: cover;
  height: 300px;
}

.coco {
  background-color: #292727;
}

.contt {
  display: flex;
  justify-content: flex-end;
  width: 400px;
  align-items: center;
}

.progress-bar {
  text-align: right;
  background-color: #545554;
  border-radius: 4px;
  transition: background-color 0.4s ease;
  color: black;
  font-weight: bolder;
}

.progress {
  border: 1px solid black;
  background-color: rgb(255, 255, 255);
  margin: 15px;
  margin-left: 35px;
  width: 200px;
}

@media screen and (max-width: 1200px) {
  .pg-2,
  .pg-3 {
    transform: none;
  }

  .pg-3 {
    margin-top: 0;
    position: relative;
    right: 105px;
  }

  .imagenPK {
    position: relative;
    width: 300px;
    height: 350px;
  }
}
@media screen and (max-width: 380px) {
  .modal-content {
    max-width: 100%;
  }
  .contt {
    flex-direction: column;
    position: relative;
    top: -90px;
  }
}
</style>
