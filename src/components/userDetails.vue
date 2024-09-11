<template>
  <div class="search_found">
    <div class="search__container">
      <div class="search__photo">
        <img src="../assets/img/noimage.png" alt="" />
      </div>
      <div class="search__data">
        <span class="search__name">{{ userData['name'] }}</span>
        <div class="search__contacts">
          <span>email:</span>
          <span>{{ userData['email'] }}</span>
        </div>
        <div class="search__contacts">
          <span>phone:</span>
          <span>{{ userData['phone'] }}</span>
        </div>
        <div class="search__about">
          <h4>О себе:</h4>
          <p>{{ userData['about'] }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      userData: {
        name: 'Ervin Howell',
        phone: '010-692-6593 x09125',
        email: 'Shanna@melissa.tv',
        photo: '',
        about:
          'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'
      }
    }
  }
}
</script>
