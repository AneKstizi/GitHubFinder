<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">

        <div class="error" v-if="error">
          <p>{{ error }}</p>
        </div>

        <search
          :value="search"
          placeholder="Type username..."
          @search="search = $event"
        />
        <button class="btn btnPrimary" @click="allReguest">Search</button>

        <loader v-if="loaderRepo"/>

        <user
          v-if="userInfo"
          :userInfo="userInfo"
        />

        <repo
          v-if="repos"
          @sort="sort"
          :repoSort="repoSort"
        />

        <div class="wrap__pagination" v-if="repos">
          <button class="btn btnPrimary btn-pagination" :class="{'btn-disabled' : page.current === 1 }" @click="prevPage">←</button>
          <button class="btn btnPrimary btn-pagination" :class="{'btn-disabled' : (page.current * page.length) >= repos.length }" @click="nextPage">→</button>
        </div>

      </div>
    </section>
  </div>
</template>

<script>
import search from '@/components/Search'
import loader from '@/components/Loader'
import repo from '@/components/Repos'
import user from '@/components/User'
import axios from 'axios'
export default {
  components: {
    search,
    loader,
    repo,
    user
  },
  data () {
    return {
      search: '',
      currentSort: 'name',
      currentSortDir: 'asc',
      page: {
        current: 1,
        length: 5
      },
      error: null,
      repos: null,
      userInfo: null,
      loaderRepo: false
    }
  },
  computed: {
    repoSort () {
      const newRepos = this.repos.slice()
      return newRepos.sort((a, b) => {
        let mod = -1
        if (this.currentSortDir === 'desc') {
          mod = 1
        }
        if (a[this.currentSort] < b[this.currentSort]) {
          return 1 * mod
        } else if (a[this.currentSort] > b[this.currentSort]) {
          return -1 * mod
        }
        return 0
      }).filter((repo, index) => {
        const start = (this.page.current - 1) * this.page.length
        const end = this.page.current * this.page.length
        if (index >= start && index < end) {
          return repo
        }
      })
    }
  },
  methods: {
    makeRequest (url) {
      return axios.get(url)
        .then(res => {
          return res.data
        })
    },

    allReguest () {
      this.loaderRepo = true
      this.repos = null
      this.userInfo = null
      Promise.all([this.makeRequest(`https://api.github.com/users/${this.search}/repos`), this.makeRequest(`https://api.github.com/users/${this.search}`)])
        .then(value => {
          [this.repos, this.userInfo] = value
          this.error = null
        })
        .catch(err => {
          console.error(err)
          this.repos = null
          this.error = 'Can`t find this user'
        })
        .finally(() => {
          this.loaderRepo = false
        })
    },
    prevPage () {
      if (this.page.current > 1) {
        this.page.current -= 1
      }
    },
    nextPage () {
      if ((this.page.current * this.page.length) < this.repos.length) {
        this.page.current += 1
      }
    },
    sort (value) {
      if (value === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
      }
      this.currentSort = value
    }
  }
}
</script>

<style lang="scss" scoped>

  .error {
    margin-bottom: 35px;
    color: rgb(216, 28, 28);
  }

  .container {
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  button {
    margin-top: 40px;
  }

  .wrap__pagination {
    width: 120px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-align: center;
  }

  .btn-pagination {
    padding: 13px;
    border-radius: 50%;
    transition: .1s background-color linear;

    &:hover {
      background-color: rgb(42, 42, 172);
    }
  }

  .btn.btn-disabled {
    background-color: rgba(51, 51, 51, .5);
    border-color:rgba(51, 51, 51, .5);
  }
</style>
