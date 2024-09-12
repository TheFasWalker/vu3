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
      userData: {
        name: 'Ervin Howell',
        phone: '010-692-6593 x09125',
        email: 'Shanna@melissa.tv',
        photo: '',
        about:
          'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'
      },
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
      console.log(searchData)
      this.loading = true
      this.error = false
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
      } finally {
        this.loading = false
        this.error = true
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
                  :key="user.name"
                  :user="user"
                  :active="index == 1 ? 'active' : ''"
                />
              </template>
            </div>
          </div>
        </div>
      </aside>
      <div v-if="searchResult" class="search_empty">
        <span class="search__empty">Выберите сотрудника, чтобы посмотреть его профиль</span>
      </div>
      <userDetails v-if="!searchResult" :userData="userData" />
    </main>
    <button @click="getUsersData">Button</button>
  </section>
</template>
