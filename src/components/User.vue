<template>
  <div class="user">
    <div class="user__info__wrapper">
      <div class="user__info__wrapper__img">
        <img class="user__info__img" :src="userInfo.avatar_url" alt="user-info">
      </div>
      <div>
        <a target="_blank" :href="userInfo.html_url" class="user__info__link">
          Name: {{ userInfo.name }}
        </a>
        <div class="user__info__location">
            Location: {{ userInfo.location }}
        </div>
        <div class="user__info__repo">
              Public repositories: {{ userInfo.public_repos }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    userInfo: {
      type: Object,
      required: true
    }
  },
  data () {
    return {

    }
  }
}
</script>

<style lang="scss" >
  .user {
    width: 400px;
    margin: 30px 0;

    &__info__wrapper {
      display: flex;
      justify-content: space-between;

      &__img {
        width: 120px;
      }
    }

    &__info__img {
      width: 100%;
      height: auto;
      border-radius: 50%;
    }

    &__info__link {
      text-decoration: none;
      color: #333;
      display: block;
      margin-top: 10px;

      &:hover {
        text-decoration: underline;
      }
    }

    &__info__location {
      margin: 15px 0;
    }
  }
</style>
