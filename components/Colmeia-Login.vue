<template>
  <div class="container">
    <form action="try-login" id="login" class="login-section">
      <img class="logo" src="../assets/images/Blanmo.svg" alt="Blanmo Login" />
      <div class="login-content">
        <div class="header-title">
          <h1>Welcome back, Marcos</h1>
          <p class="sub-title">welcome back, please enter your details</p>
          <button class="btn">
            <!-- <ion-icon name="logo-google"></ion-icon> -->
            Login with Google
          </button>

          <span class="or">or</span>
        </div>
        <div class="content-main">
          <input
            class="input-form"
            required
            autocomplete="off"
            placeholder="Email"
            v-model="userDetails.email"
          />
          <span class="errorMessage" v-if="emailError">{{ emailError }}</span>
          <input
            class="input-form"
            type="password"
            required
            autocomplete="off"
            placeholder="Password"
            v-model="userDetails.password"
          />
          <span class="errorMessage" v-if="passwordError">{{
            passwordError
          }}</span>

          <div class="remember-password">
            <div class="password-checkbox">
              <input type="checkbox" id="remember-checkbox" />
              <label for="remember-checkbox">Remember for 30 days</label>
            </div>
            <NuxtLink class="anchors" to="#"> Forgot password</NuxtLink>
          </div>
        </div>

        <div class="content-footer">
          <button
            class="content-footer-btn"
            v-on:click.prevent="handleSubmit()"
            form="login"
          >
            Log in
          </button>
          <div class="content-footer-sign-up">
            <span class="content-footer-sign-up-question"
              >Don't have an account ?</span
            >
            <NuxtLink to="#">Sign up for free</NuxtLink>
          </div>
        </div>
      </div>
    </form>
    <div class="comments-section">
      <img src="../assets/images/Blanmo.svg" alt="Blanmo Login" />
      <p class="comment">
        We move 10x faster than our peers and stay consistent. While they're
        bogged down with design debt, we're releasign new features.
      </p>
      <p class="comment">Sophie Hall</p>
      <div class="comment-pagination">
        <p class="comment-history">Founder, Catalog</p>
        <p class="comment-function">Web Design Agency</p>
      </div>
      <div class="comment-button-pagination">
        <button class="comment-button comment-button-arrow">&larr;</button>

        <button class="comment-button comment-button-arrow">&rarr;</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const urlAPI = "http://localhost:3030";

export default {
  name: "LoginScreen",
  data() {
    return {
      userDetails: {},
      emailError: "",
      passwordError: "",
    };
  },
  // marcos@gemail.com
  // 1231545
  methods: {
    async handleSubmit() {
      try {
        if (this.userDetails.email == undefined) {
          return (this.emailError = "Please type your email");
        }
        if (this.validateEmail() == false) {
          return (this.emailError = "Please type a valid email");
        }
        if (!this.userDetails.password) {
          return (this.passwordError = "This password is invalid");
        }

        const jwtToken = await axios
          .post(`${urlAPI}/auth/login`, this.userDetails)
          .then((response) => ({
            ...response,
          }));
        // localStorage.setItem("jwt", jwtToken.accessToken);
        // this.$router.push("/Colmeia-Home");
      } catch (error) {
        this.emailError = error.response.data.error;
        this.passwordError = error.response.data.error;
      }
    },
    validateEmail() {
      if (
        /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(
          this.userDetails.email
        )
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="scss">
* {
  font-family: sans-serif;
  color: #000;
  margin: 0px;
  padding: 0px;
}
html {
  background-color: #cccccc;
}
.container {
  width: 1200px;
  height: 750px;
  margin: 50px auto;
  margin-bottom: 0;
  display: flex;
  align-content: flex-start;
  background-color: white;
  justify-content: space-between;
  position: relative;
  .login-section {
    width: 33.33%;
    padding: 16.67% 8.33%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .logo {
      width: 25%;
      height: 25%;
      position: absolute;
      top: -10px;
      left: 40px;
      visibility: hidden;
    }

    .header-title {
      display: flex;
      flex-direction: column;
      text-align: center;
      justify-content: center;
      gap: 20px;
      width: 100%;

      .sub-title {
        color: #535353;
      }
      .btn {
        padding: 8px 6px;
        border: 0.5px solid #afafaf;
        border-radius: 5px;
        width: 100%;
        background-color: white;
        font-size: 16px;

        &:active {
          border: 0.5px solid #535353;
          background-color: #cccccc;
        }
      }
      .or {
        position: relative;
        &::after {
          content: "";
          width: 45%;
          background-color: #969696;
          height: 1px;
          display: block;
          position: absolute;
          top: 50%;
          left: 0;
        }

        &::before {
          content: "";
          width: 45%;
          background-color: #969696;
          height: 1px;
          display: block;
          position: absolute;
          top: 50%;
          right: 0;
        }
      }
    }
    .content-main {
      display: flex;
      flex-direction: column;
      gap: 20px;
      width: 100%;
      justify-content: center;
      margin: 35px 0;

      .errorMessage {
        font-size: 12px;
        color: red;
      }
      .input-form {
        padding: 8px 6px;
        border: none;
        border-bottom: 0.5px solid #afafaf;
      }
      .remember-password {
        display: flex;
        align-content: center;
        justify-content: space-between;
        margin-bottom: 20px;

        .anchors {
          &:visited {
            color: #000;
          }
          &:hover {
            color: #535353;
          }
          &:active {
            color: #858585;
          }
        }

        .password-checkbox {
          display: flex;
          align-content: center;
          gap: 3px;
        }
      }
    }
    .content-footer {
      display: flex;
      flex-direction: column;
      width: 100%;
      .content-footer-btn {
        padding: 8px 6px;
        border: 0.5px solid #afafaf;
        border-radius: 5px;
        width: 100%;
        color: white;
        background-color: #000;
        font-size: 14px;
        &:hover {
          background-color: #535353;
        }
        &:active {
          background-color: #858585;
        }
      }

      .content-footer-sign-up {
        display: flex;
        justify-content: center;
        gap: 5px;
        margin-top: 15px;
        .content-footer-sign-up-question {
          color: #969696;
        }
      }
    }
  }

  .comments-section {
    width: 33.33%;
    padding: 16.67% 8.33%;
    background-color: #566981;
    display: flex;
    flex-direction: column;
    justify-self: center;
    gap: 20px;
    position: relative;
    .comment {
      font-size: 220%;
      color: white;
    }

    .comment-history {
      font-size: 100%;
      color: white;
      font-weight: bold;
    }
    .comment-function {
      color: white;
    }

    .comment-pagination {
      display: flex;
      flex-direction: column;
      gap: 5px;
    }

    .comment-button {
      height: 30px;
      width: 30px;
      border: 1px solid white;
      border-radius: 50%;
      font-size: px;
      text-align: center;
      background-color: #566981;
      color: white;
    }
    .comment-button:active {
      background-color: #7892b3;
      border: #535353;
    }

    .comment-button-pagination {
      display: flex;
      justify-content: end;
      align-items: flex-end;
      gap: 20px;
      position: absolute;
      bottom: 55px;
      right: 25px;
    }
  }
}

@media (max-width: 1200px) {
  .container {
    width: 996px;
  }
}

@media (max-width: 1000px) {
  .container {
    width: 826.68px;
  }
}

@media (max-width: 800px) {
  .container {
    width: 655px;
    background-color: #969696;

    .login-section {
      width: 100%;
      .logo {
        visibility: visible;
      }
    }
    .comments-section {
      display: none;
    }
  }
}
</style>
