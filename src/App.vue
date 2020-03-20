<template>
    <div id="app">
        <h1>PokeGu&iacute;a</h1>
        Nombre: <input id="nombrePokemon" value="" /> <button @click="obtengoDataPokemonDesdeBoton()">Buscar</button><br />
        <img :src="imagenPokemon" />
        <h2>Movimientos</h2>
        <div class="div__movimientos">
            <p v-for="movimiento in movimientosPokemon">{{ movimiento.move.name }}</p>
        </div>
        <h2>Habilidades</h2>
        <div class="div__habilidades">
            <p v-for="habilidad in habilidadesPokemon">{{ habilidad.stat.name }}</p>
        </div>
    </div>
    </template>
    <script>
        export default {
            data() {
                return {
                    imagenPokemon: '',
                    nombrePokemon: 'pikachu',
                    datosPokemon: '',
                    movimientosPokemon: '',
                    habilidadesPokemon: ''
                };
            },
            methods: {
                obtengoDataPokemonDesdeBoton() {
                    let nombre = $("#nombrePokemon").val();
                    this.obtengoDataPokemon(nombre);
                },
                obtengoDataPokemon(nombre) {                  
                    let data;
                    fetch("https://pokeapi.co/api/v2/pokemon/" + nombre)
                        .then(response => response.json())
                        .then(data => data)
                        .then(res => {
                            data = res;
                            this.datosPokemon = data;
                            return data;
                        }).then(res => {
                            this.obtieneDetallePokemon();
                        }).catch(error => console.log(error));
                }
            },
            created() {
                let nombre = this.nombrePokemon;
                this.obtengoDataPokemon(nombre);
            },
            computed: {
                obtieneDetallePokemon() {
                    //obtengo imagen
                    let data = this.datosPokemon;
                    this.imagenPokemon = data.sprites.front_default;
                    //obtengo movimientos
                    this.movimientosPokemon = data.moves;
                    //obtengo habilidades
                    this.habilidadesPokemon = data.stats;
                }               
            }
        };
    </script>
    <style>
        #app{
            text-align: center;
        }
        .div__movimientos{
            display: grid;
            grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
        }
        .div__habilidades{
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
        }
    </style>
