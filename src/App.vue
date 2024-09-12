<script>
import inputComponent from './components/inputComponen.vue'
import userPreview from './components/userPreview.vue'
import userDetails from './components/userDetails.vue'
import loderComponent from './components/loderComponent.vue'

export default {
  data() {
    return {
      loading: false,
      error: false,
      userDataLoading: false,
      userDataLoadingError: false,
      userData: '',
      usersData: []
    }
  },
  components: {
    inputComponent: inputComponent,
    userPreview: userPreview,
    userDetails: userDetails,
    loderComponent: loderComponent
  },
  methods: {
    onEndInput: async function () {
      let searchData = event.target.value
      this.loading = true
      this.error = false
      this.userData = ''
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users?username_like=${searchData}`
        )
        if (!response.ok) {
          throw new Error('Network response was not ok')
        }
        this.usersData = await response.json()
      } catch (error) {
        this.error = 'Error fetching users: ' + error.message
        this.error = true
      } finally {
        this.loading = false
        this.error = true
      }
    },

    async clickElem(index, id) {
      this.usersData.forEach((user, i) => {
        user.isActive = i === index
      })

      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users?id=${id}`)
        if (!response.ok) {
          throw new Error('Network response was not ok')
        }

        const userDataArray = await response.json()
        this.userData = userDataArray[0]
      } catch (error) {
        this.userDataLoadingError = 'Error fetching user by id: ' + error.message
      } finally {
        this.userDataLoading = false
        this.userDataLoadingError = true
      }
    }
  }
}
</script>

<template>
  <section class="container">
    <header>
      <a class="logo" href="#">Жилфонд</a>
      <span>Пользователь</span>
    </header>
    <main>
      <aside class="aside">
        <div class="aside__container">
          <h2 class="aside__title">Поиск сотрудников</h2>
          <inputComponent :onEndInput="onEndInput" />
          <div class="aside__results">
            <h3 class="aside__title">Результаты</h3>
            <template v-if="loading">
              <loderComponent />
            </template>
            <span v-if="usersData.length < 1 && !error" class="aside__subtitle"
              >начните поиск
            </span>
            <span v-if="usersData.length < 1 && error" class="aside__subtitle"
              >нет соответствий
            </span>
            <div class="aside__content">
              <template v-if="usersData.length >= 1">
                <userPreview
                  v-for="(user, index) of usersData"
                  :key="user.id"
                  :user="user"
                  :active="user.isActive"
                  @click="clickElem(index, user.id)"
                />
              </template>
            </div>
          </div>
        </div>
      </aside>
      <div v-if="!userData" class="search_empty">
        <span class="search__empty">Выберите сотрудника, чтобы посмотреть его профиль</span>
        <template v-if="userDataLoading">
          <loderComponent />
        </template>
      </div>
      <userDetails v-if="userData" :userData="userData" />
    </main>
  </section>
</template>
