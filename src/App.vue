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
    },
    clickElem(index) {
      this.usersData.forEach((user, i) => {
        user.isActive = i === index 
      })
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
                  :active="user.isActive"
                  @click="clickElem(index)"
                />
              </template>
            </div>
          </div>
        </div>
      </aside>
      <div v-if="!userData.length" class="search_empty">
        <span class="search__empty">Выберите сотрудника, чтобы посмотреть его профиль</span>
      </div>
      <userDetails v-if="userData.length" :userData="userData" />
    </main>
  </section>
</template>
