<template>
  <v-app>
    <v-main>
      <div>
        <v-snackbar top v-model="snackBar.show" timeout="2000">
          {{ snackBar.text }}

          <template v-slot:actions>
            <v-btn color="blue" variant="text" @click="snackBar.show = false">
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
      <div v-if="!isLoggedIn">
        <LoginSignUpComponent
          @userLoggedIn="onUserLoggedIn"
          @registeredUser="onRegisteredUser"
        />
      </div>
      <div v-else>
        <DashboardComponent :user="user" />
      </div>
    </v-main>
  </v-app>
</template>

<script>
import LoginSignUpComponent from "./components/LoginSignUpComponent";
import DashboardComponent from "./components/DashboardComponent";

export default {
  name: "App",

  components: {
    LoginSignUpComponent,
    DashboardComponent,
  },

  data: () => ({
    isLoggedIn: false,
    user: null,
    snackBar: {
      show: false,
      text: "",
    },
  }),

  methods: {
    onUserLoggedIn(responseData) {
      this.user = responseData.data;
      this.isLoggedIn = true;
      this.snackBar.show = true;
      this.snackBar.text = responseData.message;
    },
    onRegisteredUser(responseData) {
      this.snackBar.show = true;
      this.snackBar.text = responseData.message;
    },
  },
};
</script>
