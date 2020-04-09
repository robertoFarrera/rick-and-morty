<template>
  <div class="column is-narrow">
    <div class="card">
      <div class="card-header">
        <img v-bind:src="character.image" v-bind:alt="character.name">
      </div>
      <div class="card-content">
        <h2 class="title is-size-6 is-size-5-tablet truncate">{{ character.name }}</h2>
        <button
          class="button is-small is-rounded is-primary"
          @click="showMore(character.id)"
        >Ver más</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["character"],
  methods: {
    showMore(id) {
      this.$emit("showModal", id);
    }
  }
};
</script>

<style>
.truncate {
  line-height: 1.5;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.card {
  border: solid 1px rgb(197, 219, 80);
  box-shadow: 0 0.5em 1em -0.125em rgba(104, 195, 112, 0.7),
    0 0px 0 1px rgba(104, 195, 112, 0.5);
}
.button.is-primary {
  background-color: #00afc8;
  box-shadow: 0 0.5em 1em -0.125em rgba(104, 195, 112, 0.7),
    0 0px 0 1px rgba(104, 195, 112, 0.5);
}
.button.is-primary:hover {
  background-color: #039db4;
}
</style>