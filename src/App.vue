<template>
  <b-container id="app" fluid>
    <b-row v-if="!isLogged" id="login-component" class="full-screen-component">
      <login></login>
    </b-row>
    <b-row id="chat-component" class="full-screen-component">
      <b-row class="app-one">
        <b-col cols="3" class="side">
          <div class="side-left">
            <b-row class="heading">
              <b-col cols="2" sm="2" class="heading-avatar align-self-center">
                <div class="heading-avatar-icon">
                  <font-awesome-icon @click="showProfile" icon="bars" />
                </div>
              </b-col>

              <b-col cols="10" sm="10" class="heading-name">
                <a class="heading-name-meta">{{ appUser.name }}</a>
              </b-col>
            </b-row>

            <b-row class="m-0 h-100">
              <b-col id="room-list-wrapper" class="side">
                <room-list></room-list>
              </b-col>
            </b-row>
          </div>
          <div class="side-profile">
            <profile></profile>
          </div>
        </b-col>

        <b-col cols="9" class="room">
          <b-row class="heading">
            <b-col cols="12" class="heading-name">
              <a v-if="selectedRoomTitle" class="heading-name-meta">{{
                selectedRoomTitle
              }}</a>
            </b-col>
          </b-row>

          <b-row id="room-box" class="m-0">
            <room-history></room-history>
          </b-row>
        </b-col>
      </b-row>
    </b-row>
  </b-container>
</template>

<script>
import { mapGetters, mapState } from "vuex";
import Login from "./components/Login.vue";
import Profile from "./components/Profile.vue";
import RoomList from "./components/RoomList.vue";
import RoomHistory from "./components/RoomHistory.vue";
export default {
  name: "App",
  components: {
    Login,
    Profile,
    RoomList,
    RoomHistory,
  },
  computed: {
    ...mapState({
      appUser: (state) => state.appUser,
      selectedRoom: (state) => state.selectedRoom,
    }),
    ...mapGetters({
      isLogged: "isLogged",
      appReady: "appReady",
    }),
    selectedRoomTitle: function () {
      if (this.selectedRoom != null) {
        return this.selectedRoom.name;
      }
      return "";
    },
  },
  methods: {
    showLogin() {
      var div = document.getElementById("login-component");
      div.style.left = "0%";
    },
    hideLogin() {
      var div = document.getElementById("login-component");
      div.style.left = "-100%";
    },
    showProfile() {
      var div = document.getElementsByClassName("side-profile")[0];
      div.style.left = "0%";
    },
  },
  mounted() {},
};
</script>
<style>
@import "./assets/css/chat.css";
.app-one {
  width: 100%;
}
#room-box {
  height: calc(100% - 60px);
}
#app {
  text-align: center;
  color: #2c3e50;
  height: 100%;
  width: 100%;
  overflow: hidden;
  margin: 0;
  padding: 0;
  position: fixed;
  /*position: relative;
  height: calc(100% - 52px);*/
}
#chat-component {
  z-index: 1;
}

#login-component {
  z-index: 99999;
  left: 0;
  position: absolute;
  -webkit-transition: left 2s ease;
  transition: left 2s ease;
}

.full-screen-component {
  height: 100%;
  width: 100%;
  padding: 0;
  margin: 0;
}

#history {
  display: inline-block;
}

#room-list-wrapper {
  overflow-y: auto;
  height: calc(100% - 60px);
}

.heading-avatar-icon > svg {
  height: 100%;
  width: 100%;
}
</style>
