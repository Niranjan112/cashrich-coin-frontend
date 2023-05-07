<template>
  <v-container>
    <div>
      <v-tabs
        v-model="tab"
        show-arrows
        background-color="deep-purple accent-4"
        icons-and-text
        dark
        grow
      >
        <v-tabs-slider color="purple darken-4"></v-tabs-slider>
        <v-tab v-for="i in tabs" :key="i.name">
          <v-icon large>{{ i.icon }}</v-icon>
          <div class="caption py-1">{{ i.name }}</div>
        </v-tab>
        <v-tab-item>
          <v-card class="px-4">
            <v-card-text>
              <v-form ref="loginForm" v-model="loginFormValid" lazy-validation>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      v-model="loginUserName"
                      :rules="userNameRules"
                      label="User Name"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="loginPassword"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="passwordRules"
                      :type="show1 ? 'text' : 'password'"
                      name="input-10-1"
                      label="Password"
                      hint="At least 8 characters"
                      counter
                      @click:append="show1 = !show1"
                    ></v-text-field>
                  </v-col>
                  <v-col class="d-flex" cols="12" sm="6" xsm="12"> </v-col>
                  <v-spacer></v-spacer>
                  <v-col class="d-flex" cols="12" sm="3" xsm="12" align-end>
                    <v-btn
                      x-large
                      block
                      :disabled="!loginFormValid"
                      color="success"
                      @click="login"
                    >
                      Login
                    </v-btn>
                  </v-col>
                </v-row>
              </v-form>
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card class="px-4">
            <v-card-text>
              <v-form
                ref="registerForm"
                v-model="signUpFormValid"
                lazy-validation
              >
                <v-row>
                  <v-col cols="12" sm="6" md="6">
                    <v-text-field
                      v-model="firstName"
                      :rules="[rules.required, rules.onlyLetter]"
                      label="First Name"
                      maxlength="50"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="6">
                    <v-text-field
                      v-model="lastName"
                      :rules="[rules.required, rules.onlyLetter]"
                      label="Last Name"
                      maxlength="50"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="userName"
                      :rules="userNameRules"
                      label="User Name"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      v-model="password"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="passwordRules"
                      :type="show1 ? 'text' : 'password'"
                      name="input-10-1"
                      label="Password"
                      hint="At least 8 characters"
                      counter
                      @click:append="show1 = !show1"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      block
                      v-model="verify"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="[rules.required, passwordMatch]"
                      :type="show1 ? 'text' : 'password'"
                      name="input-10-1"
                      label="Confirm Password"
                      counter
                      @click:append="show1 = !show1"
                    ></v-text-field>
                  </v-col>
                  <v-spacer></v-spacer>
                  <v-col class="d-flex ml-auto" cols="12" sm="3" xsm="12">
                    <v-btn
                      x-large
                      block
                      :disabled="!signUpFormValid"
                      color="success"
                      @click="signup"
                      >Register</v-btn
                    >
                  </v-col>
                </v-row>
              </v-form>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs>
    </div>
  </v-container>
</template>

<script>
// import axios from "axios";
// import { headers } from "../shared/default-header";

export default {
  name: "LoginSignUpComponent",
  data() {
    return {
      tab: 0,
      tabs: [
        { name: "Login", icon: "mdi-account" },
        { name: "Register", icon: "mdi-account-outline" },
      ],
      loginFormValid: false,
      signUpFormValid: false,
      firstName: "",
      lastName: "",
      userName: "",
      password: "",
      verify: "",
      loginPassword: "",
      loginUserName: "",
      userNameRules: [
        (v) => !!v || "Required",
        (v) => /^[a-zA-Z0-9]+$/.test(v) || "Only letter and digits allowed",
        (v) => v.length > 4 || "Atleast 4 characters required",
        (v) => v.length <= 15 || "Upto 15 characters allowed",
      ],
      passwordRules: [
        (v) =>
          /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*()_+~`|}{[\]\\:';"<>?,./-]).*$/.test(
            v
          ) ||
          "At least 1 upper, 1 lower, 1 digit and 1 special character required",
        (value) => !!value || "Required.",
        (v) => (v && v.length >= 8) || "Min 8 characters",
        (v) => (v && v.length <= 15) || "Max 15 characters only",
      ],
      rules: {
        required: (v) => !!v || "Required",
        onlyLetter: (v) => /^[a-zA-Z]+$/.test(v) || "Only letters allowed",
      },
      show1: false,
    };
  },
  computed: {
    passwordMatch() {
      return () => this.password === this.verify || "Password must match";
    },
  },
  methods: {
    async signup() {
      if (this.$refs.registerForm.validate()) {
        const user = {
          firstName: this.firstName,
          lastName: this.lastName,
          userName: this.userName,
          password: this.password,
        };

        const response = await fetch("http://localhost:8081/api/user", {
          method: "POST",
          body: JSON.stringify(user),
          headers: {
            "Content-Type": "application/json",
            "origin-api-key": "23232-2323fdefe-3343f",
          },
        });

        const jsonData = await response.json();

        this.$emit("registeredUser", jsonData);
      }
    },
    async login() {
      if (this.$refs.loginForm.validate()) {
        const userCred = {
          userName: this.loginUserName,
          password: this.loginPassword,
        };

        const response = await fetch("http://localhost:8081/api/user/login", {
          method: "POST",
          body: JSON.stringify(userCred),
          headers: {
            "Content-Type": "application/json",
            "origin-api-key": "23232-2323fdefe-3343f",
          },
        });

        const jsonData = await response.json();

        this.$emit("userLoggedIn", jsonData);
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
  },
};
</script>
