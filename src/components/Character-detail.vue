<template>
  <article class="media box is-radiusless">
    <figure class="media-left is-hidden-mobile">
      <p class="image is-64x64">
        <img v-bind:src="mc.image">
      </p>
    </figure>
    <div class="media-content">
      <div class="content">
        <p>
          <strong>{{mc.name}}</strong>
          <small :class="tagClass" class="tag">{{mc.status}}</small>
        </p>
        <ul>
          <li :class="typeClass">
            <strong>Tipo:</strong>
            {{mc.type}}
          </li>
          <li>
            <strong>Especie:</strong>
            {{mc.species}}
          </li>
          <li>
            <strong>Género:</strong>
            {{mc.gender}}
          </li>
          <li>
            <strong>Origen:</strong>
            {{mc.origin.name}}
          </li>
          <li>
            <strong>Ubicacion:</strong>
            {{mc.location.name}}
          </li>
        </ul>
      </div>
      <nav class="level is-mobile">
        <div class="level-left">
          <a class="level-item">
            <span class="icon is-small">
              <i class="fas fa-reply"></i>
            </span>
          </a>
          <a class="level-item">
            <span class="icon is-small">
              <i class="fas fa-retweet"></i>
            </span>
          </a>
          <a class="level-item">
            <span class="icon is-small">
              <i class="fas fa-heart"></i>
            </span>
          </a>
        </div>
      </nav>
    </div>
    <div class="media-right">
      <button @click="closeModal" class="delete"></button>
    </div>
  </article>
</template>

<script>
export default {
  props: ["mc"],
  data: function() {
    return {
      tagClass: "is-warning",
      typeClass: "is-hidden"
    };
  },
  created() {
    if (this.mc.status.localeCompare("Alive") === 0) {
      this.tagClass = "is-success";
    }
    if (this.mc.status.localeCompare("Dead") === 0) {
      this.tagClass = "is-danger";
    }
    if (!this.mc.type.isNull()) {
      this.typeClass = "";
    }
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    }
  }
};
</script>

<style>
.tag {
  margin-left: 0.5rem;
}
.box,
.image {
  border: solid 1px rgb(197, 219, 80);
  box-shadow: 0 0.5em 1em -0.125em rgba(104, 195, 112, 0.7),
    0 0px 0 1px rgba(104, 195, 112, 0.5);
}
</style>