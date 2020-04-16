<template>
  <div class="repos__wrapper">
    <div class="repos__wrapper__sort">
      <div class="repos__sort" @click="$emit('sort', 'name')">Name &#8595; &#8593;</div>
      <div class="repos__sort" @click="$emit('sort', 'stargazers_count')">⭐ &#8595; &#8593;</div>
    </div>
    <div class="repos-item" v-for="repo in repoSort" :key="repo.id">
      <div class="repos-info">
        <a target="_blank" :href="repo.html_url">{{ repo.name }} </a>
          <span>{{ repo.stargazers_count }} ⭐</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    repoSort: {
      type: Array,
      required: true
    }
  },
  data () {
    return {

    }
  }
}
</script>

<style lang="scss" scoped>
.repos__wrapper {
    width: 400px;
    margin: 30px 0;

    &__sort {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 7px;
      padding: 7px;
      border-bottom: 1px solid #333;
    }
  }
  .repos-info {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
    padding: 10px 0;
    border-bottom: 1px solid #dbdbdb;
  }

  .repos__sort {
    cursor: pointer;
  }
</style>
