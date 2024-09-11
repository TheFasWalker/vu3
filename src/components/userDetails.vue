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
  props: {
    userData: {
      type: Object,
      required: true
    }
  }
}
</script>
