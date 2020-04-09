<template>
  <nav class="pagination is-centered is-rounded is-small" role="navigation" aria-label="pagination">
    <ul class="pagination-list">
      <li :class="page<3 ? 'is-hidden':''">
        <a @click="changePage(1)" class="pagination-link" aria-label="Page 1" aria-current="page">1</a>
      </li>
      <li :class="page<4 ? 'is-hidden':''">
        <span class="pagination-ellipsis">&hellip;</span>
      </li>
      <li :class="page<2 ? 'is-hidden':''">
        <a @click="changePage(page-1)" class="pagination-link">{{page-1}}</a>
      </li>
      <li>
        <a class="pagination-link is-current">{{page}}</a>
      </li>
      <li :class="page>=pages ? 'is-hidden':''">
        <a @click="changePage(page+1)" class="pagination-link">{{page+1}}</a>
      </li>
      <li :class="page>pages-3 ? 'is-hidden':''">
        <span class="pagination-ellipsis">&hellip;</span>
      </li>
      <li :class="page>pages-2 ? 'is-hidden':''">
        <a
          @click="changePage(pages)"
          class="pagination-link"
          aria-label="Page 46"
          aria-current="page"
        >{{pages}}</a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  props: ["page", "pages"],
  methods: {
    changePage(page) {
      let currentPage = page >= 1 && page <= this.pages ? page : this.page;
      this.$emit("changePage", currentPage);
    }
  }
};
</script>

<style>
.pagination-link.is-current {
  background-color: #00afc8;
  border-color: #00afc8;
}
</style>
