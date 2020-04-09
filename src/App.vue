<template>
  <div id="app">
    <div class="hero is-dark is-bold has-text-centered-mobile">
      <div class="hero-body">
        <h1 class="title">
          <img
            src="https://help.redbubble.com/hc/article_attachments/360002309526/Rick_and_Morty_-_logo__English_.png"
            alt
          >
          <span class="subtitle is-family-monospace">Personajes</span>
        </h1>
        <p>Este proyecto se desarrolló con Vue como framework de JavaScript y Bulma como framework CSS.</p>
        <p
          class="content"
        >El objetivo es implementar axios como librería de Vue para consumir la API:</p>
        <div class="columns">
          <div class="column">
            <span class="tag is-medium is-white is-rounded">
              <a href="https://rickandmortyapi.com/">https://rickandmortyapi.com/</a>
            </span>
          </div>
          <div class="column is-narrow">
            <div class="field has-addons">
              <div class="control has-text-black">
                <input
                  type="text"
                  class="input is-rounded"
                  v-model="search"
                  @keyup.enter="searchData"
                >
              </div>
              <div class="control">
                <button class="button is-primary is-rounded" @click="searchData">Buscar</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="section container">
      <div :class="notification.class" class="notification">
        <button class="delete" @click="notification.class='is-hidden'"></button>
        <strong>{{notification.type}}:</strong>
        {{notification.msg}}.
      </div>
      <div class="columns is-mobile is-multiline is-centered">
        <character
          @showModal="showModal"
          v-for="character of characters"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </div>
    </div>
    <div :class="characters.length<1 ? 'is-hidden':''" class="section">
      <navegacion v-bind:pages="pages" v-bind:page="page" @changePage="changePage"/>
    </div>
    <div v-if="modal" class="modal" :class="modal ? 'is-active':''">
      <div class="modal-background"></div>
      <div class="modal-content has-background-white">
        <modal v-bind:mc="modalCharacter" @closeModal="closeModal"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Character from "./components/Character";
import Navegacion from "./components/Navegacion";
import Modal from "./components/Character-detail";

export default {
  name: "App",
  components: {
    Character,
    Navegacion,
    Modal
  },
  created() {
    this.fetch();
  },
  data: function() {
    return {
      notification: {
        class: "is-hidden",
        type: "",
        msg: ""
      },
      characters: [],
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      modalCharacter: {}
    };
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search
      };

      axios
        .get("https://rickandmortyapi.com/api/character", { params })
        .then(res => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
        })
        .catch(error => {
          if (error.response) {
            this.characters = [];
            this.page = 1;
            this.notification = {
              class: "is-danger",
              type: "Error " + error.response.status,
              msg: error.response.data.error
            };
          }
        });
    },
    changePage(page) {
      this.page = page;
      this.fetch();
    },
    searchData() {
      this.page = 1;
      this.notification = {
        class: "is-primary",
        type: "Resultados de búsqueda:",
        msg: this.search
      };
      this.fetch();
      this.search = "";
    },
    async showModal(id) {
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}`
      );

      this.modalCharacter = result.data;
      this.modal = true;
    },
    closeModal() {
      this.modal = false;
    }
  }
};
</script>

<style>
.button.is-primary {
  background-color: #00afc8;
  border: solid 1px rgb(197, 219, 80);
  box-shadow: 0 0.5em 1em -0.125em rgba(104, 195, 112, 0.7),
    0 0px 0 1px rgba(104, 195, 112, 0.5);
}
.button.is-primary:hover {
  background-color: #039db4;
}
</style>
