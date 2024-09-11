<template>
  <div class="user" :class="active">
    <div class="user__photo">
      <img src="../assets/img/noimage.png" alt="" />
    </div>
    <div class="user__data">
      <span>{{ user['username'] }}</span>
      <span>{{ user['email'] }}</span>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    user: {
      type: Object
    },
    active: String
  }
}
</script>
<style lang="scss">
.user {
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
  background: #fff;
  border-radius: 10px;
  display: grid;
  grid-template-columns: 70px 1fr;
  margin-bottom: 18px;
  margin-right: 5px;
  margin-left: 5px;
  height: 70px;
  cursor: pointer;
  &__data {
    display: flex;
    flex-direction: column;
    padding: 15px;
    gap: 5px;
    border-radius: 0 10px 10px;
    span:first-child {
      font-weight: 600;
      font-size: 14px;
      color: #333;
    }
    span:last-child {
      font-weight: 400;
      font-size: 12px;
      color: #76787d;
    }
  }
  &__photo {
    overflow: hidden;
    border-right: 1px solid #e0e0e0;
    border-radius: 10px 0 0 10px;
  }
  &.active {
    box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
    background: #e0e0e0;
    .user__data {
      background: #e0e0e0;
    }
  }
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
