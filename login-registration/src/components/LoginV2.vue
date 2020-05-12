<template>
  <div class="body" v-if="!authorized">
    <q-card class="main">
      <div class="cardImage">
        <div class="content">
          <div
            class="full-width q-my-md q-pa-sm cta cta__facebook text-center text-white"
            @click="processGoogleLogin"
          >Login With Google</div>
          <div
            class="full-width q-pa-sm cta cta__facebook text-center text-white"
            @click="processFacebookLogin"
          >Login With Facebook</div>
        </div>
      </div>
      <div class="container q-pb-md">
        <div class="container__toggle_btn flex">
          <div class="socialLogin flex q-pa-lg">
            <div class="btn google q-px-md q-py-sm" @click="processGoogleLogin">Google Login</div>
            <div class="btn fb q-px-md q-py-sm" @click="processFacebookLogin">Facebook Login</div>
          </div>

          <div class="flex q-pa-lg">
            <div
              class="btn q-px-md q-py-sm"
              :class="{active: authPanel === constants.login}"
              @click="() => {toggleAuth(constants.login)}"
            >{{ constants.login }}</div>
            <div
              class="btn q-px-md q-py-sm"
              :class="{active: authPanel === constants.register}"
              @click="() => {toggleAuth(constants.register)}"
            >{{ constants.register }}</div>
          </div>
        </div>
        <div class="q-my-md q-mx-xl flex justify-center container__input_area">
          <q-card flat class="section q-mb-md q-px-lg full-width">
            <div>
              <span class="title text-blue-grey-9">{{ authPanel }}</span>
            </div>
            <span class="text-blue-6">{{ status }}</span>
            <div class="input__container">
              <div v-if="authPanel === constants.register">
                <q-input
                  v-model="name"
                  label="Full Name"
                  placeholder="Jhon Doe"
                  color="light-blue-7"
                  ref="name"
                  :rules="[ val => val && val.length > 0 || 'Enter your full name']"
                />
              </div>
              <div>
                <q-input
                  v-model="identifier"
                  class="input"
                  label="Email"
                  placeholder="someone@example.com"
                  color="light-blue-7"
                  ref="email"
                  :rules="[ val => val && val.length >= 5 || 'Enter your most used email']"
                />
              </div>
              <div>
                <q-input
                  v-model="password"
                  :type="isPwd ? 'password' : 'text'"
                  label="Password"
                  placeholder="••••••••"
                  color="light-blue-7"
                  ref="password"
                  :rules="[val => val && val.length >= 4 || 'Please enter password' ]"
                >
                  <template v-slot:append>
                    <q-icon
                      :name="isPwd ? 'visibility_off' : 'visibility'"
                      class="cursor-pointer"
                      @click="isPwd = !isPwd"
                    />
                  </template>
                </q-input>
                <template v-if="authPanel===constants.login">
                  <div
                    class="full-width q-mt-xl q-pa-md cta text-center text-white"
                    @click="processLogin"
                  >login</div>
                  <div class="full-width flex q-mt-sm q-pt-md cta__group">
                    <span class="cta-label text-blue-grey-7">Forgot Password?</span>
                    <span
                      class="cta-label text-blue-grey-7"
                      @click="toggleAuth(constants.register)"
                    >Not a member yet?</span>
                  </div>
                </template>
                <template v-else>
                  <div
                    class="full-width q-mt-xl q-pa-md cta text-center text-white"
                    @click="processRegister"
                  >register</div>
                  <div class="full-width flex q-mt-sm q-pt-md cta__group">
                    <span class="cta-label text-blue-grey-7">Forgot Password?</span>
                    <span
                      class="cta-label text-blue-grey-7"
                      @click="toggleAuth(constants.login)"
                    >Already a member?</span>
                  </div>
                </template>
              </div>
            </div>
          </q-card>
        </div>
      </div>
    </q-card>
  </div>
  <div v-else>
    <q-card class="flex authorized">
      <h4>Authorized</h4>
    </q-card>
  </div>
</template>

<script>
export default {
  mounted() {},
  data() {
    return {
      authPanel: "Login",
      server: "http://locahost:1337/",
      isPwd: true,
      name: "",
      identifier: "",
      password: "",
      constants: {
        login: "Login",
        register: "Register"
      },
      status: "",
      authorized: false
    };
  },
  methods: {
    toggleAuth(authPanel) {
      this.authPanel = authPanel;
    },
    checkEmail(v) {
      const pattern = /^\w+([._-]?\w+)*@\w+([._-]?\w+)*(\.\w{2,5})+$/;
      return pattern.test(v);
    },
    processRegister() {
      this.$refs.name.validate();
      this.$refs.email.validate();
      this.$refs.password.validate();
      if (
        !this.$refs.name.hasError &&
        !this.$refs.email.hasError &&
        !this.$refs.password.hasError
      ) {
        const dataObj = {
          name: this.name,
          username: this.identifier,
          email: this.identifier,
          password: this.password
        };
        console.log("register ->", dataObj);
        this.status = "Account has been created, please proceed to login";
        this.authPanel = this.constants.login;
      }
    },
    processLogin() {
      this.$refs.email.validate();
      this.$refs.password.validate();
      if (!this.$refs.email.hasError && !this.$refs.password.hasError) {
        var dataObj = {
          identifier: this.identifier,
          password: this.password
        };
        console.log("login ->", dataObj);
        this.authorized = true;
      }
    },
    processFacebookLogin() {
      let url = this.server + "connect/facebook";
      window.location = url;
    },
    processGoogleLogin() {
      let url = this.server + "connect/facebook";
      window.location = url;
    }
  }
};
</script>

<style lang="stylus" scoped>
.socialLogin
  visibility hidden

.body
  min-height 100vh
  max-height 100%
  background url('../assets/bg0.png') no-repeat
  background-position center
  width 100%
  transition 0.3s
  background-size cover
  display: flex
  justify-content center
  align-items center

.main
  color black
  display grid
  grid-template-columns 50% 50%
  min-height 55vh
  width 75vw
  transition 0.3s

.cardImage
  background url('../assets/SidePanel2.png') no-repeat
  background-size cover

.cardImage .content
  width 50%
  position absolute
  bottom 5rem
  display flex
  flex-direction column
  justify-content center
  align-items center

.title
  font-size 30px

.btn
  cursor pointer
  background-color #eee2e2
  font-family 'Roboto'
  &:hover
    background-color darken(#cfcfcf, 10%)

.active
  color: white
  background-color #427bec
  &:hover
    background-color darken(#427bec, 10%)

.fb, .google
  color: white

.fb
  background: #3d6fd5
  &:hover
    background darken(#3d6fd5, 10%)

.google
  background #ff6f7c
  &:hover
    background darken(#ff6f7c, 10%)

.input__container
  margin-top 15px

.section span
  font-family 'Roboto'
  font-weight 700

.container__toggle_btn
  justify-content flex-end

.cta
  cursor pointer
  background-color #427bec
  font-family 'Roboto'
  font-weight 700
  text-transform uppercase
  letter-spacing 2px
  &:hover
    background-color darken(#427bec, 10%)

.cta__facebook
  border 1px solid white
  max-width 50%
  background-color #3d6fd5
  &:hover
    background-color darken(#3d6fd5, 10%)

.cta__group
  display flex
  justify-content space-between

.cta-label
  cursor pointer

.authorized
  display flex
  justify-content center
  align-items center
  height 100vh
  width 100vw

@media only screen and (max-width: 1234px)
  .main
    grid-template-columns 100%
  .cardImage
    transition 0.3s
    display none
  .container__toggle_btn
    justify-content: space-between
  .socialLogin
    visibility visible

@media only screen and (max-width: 704px)
  .cta__group
    flex-direction column
    justify-content center
    align-items center
  .body
    align-items center
  .input
    font-size 12px
  .cta-label
    margin 10px 0
    font-size 12px

@media only screen and (max-width: 704px)
  .container__toggle_btn
    flex-direction column
    justify-content center
    align-items center

@media only screen and (max-width: 415px)
  .container__input_area
    padding 10px 0
    margin 10px 0
</style>
