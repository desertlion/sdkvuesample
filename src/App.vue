<script>
import QiscusSDK from 'qiscus-sdk-core';

export default {
  data: () => ({
    is_login: false,
    selected: null,
    user_data: null,
    rooms: [],
    qiscus: new QiscusSDK(),
  }),
  mounted() {
    const self = this;
    this.qiscus.init({
      AppId: 'sdksample',
      options: {
        loginSuccessCallback(data) {
          console.log('succes', data)
          self.is_login = true;
          self.user_data = self.qiscus.userData;
          // load the rooms
          self.qiscus.loadRoomList()
            .then(res => self.rooms = res)
        }
      }
    });
    this.qiscus.setUser('fikri@qiscus.com', 'password', 'Fikri (Qiscus)');
  },
}
</script>

<template>
  <div v-if="is_login">
    <div>Welcome, {{user_data.username}}</div>
    <div v-if="!rooms">Loading rooms ...</div>
    <div v-if="rooms">
      <ul>
        <li v-for="room in rooms" :key="room.id">
          {{  room.name }}
        </li>
      </ul>
    </div>
  </div>
  <div v-else>Please login first</div>
</template>

<style scoped>
</style>
