<template>
  <div class="login">
    <div class="login__content">
      <div class="login__content__textbox">
        <h1>누구나 퍼블리싱을 손쉽게</h1>
        <p>
          복잡하고 번거로운 퍼블리싱 툴 대신 셀 단위의 사용자간 실시간 편집을 통해
          <br />새로운 차원의 퍼블리싱을 경험해보세요
        </p>
      </div>
      <img class="image" src="@/assets/CoTeX.svg" />
    </div>
    <div class="login__form">
      <div class="login__form__header">
        <img src="@/assets/logo.svg" alt="logo" class="logo" />
        <h2>{{ !isRegister ? "로그인" : "가입하기" }}</h2>
        <p>CoTeX에서 손쉽게 출판을 시작해보세요.</p>
      </div>

      <div class="login__form__social">
        <h2>SNS 로그인</h2>
        <Button class="button login__form__social__naver" @click="naverLogin">
          <img src="https://recruit.navercorp.com/img/favicon/naver_favicon.ico" alt="Naver Logo" width="28" height="28" /> 네이버로 로그인하기
        </Button>
        <Button class="button login__form__social__kakao" @click="kakaoLogin"> <img src="@/assets/kakao.png" alt="Kakao Logo" width="28" height="28" /> 카카오로 로그인하기 </Button>
        <Button class="button login__form__social__github" @click="githubLogin"> <i class="iconify github" data-icon="mdi-github"></i> GitHub로 로그인하기 </Button>
      </div>
      <hr />
      <form action="javascript:void(0)" @submit="submit" class="login__form__loginbox">
        <label class="inputbox">
          <div class="login__label">아이디 &ast;</div>
          <input class="textinput" v-model="id" placeholder="ID" required />
        </label>
        <label class="inputbox">
          <div class="login__label">비밀번호 &ast;</div>
          <input class="passwordinput" v-model="password" placeholder="Password" required type="password" />
        </label>
        <Button class="login__from__submitBtn" type="submit">{{ !isRegister ? "로그인" : "가입하기" }}</Button>
      </form>
      <div v-if="!isRegister" class="login__form__first">
        CoTex에 처음인가요?
        <b
          @click="
            () => {
              isRegister = true;
            }
          "
          >가입하기</b
        >
      </div>
      <div v-else class="login__form__first">
        이미 계정이 있으신가요?
        <b
          @click="
            () => {
              isRegister = false;
            }
          "
          >로그인</b
        >
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

import Button from "@/components/Button.vue";

@Component({
  components: {
    Button,
  },
})
export default class Login extends Vue {
  isRegister: boolean = false;
  id: string = "";
  password: string = "";

  naverLogin() {
    window.location.href = "https://cotex.hyunwoo.dev/api/auth/user/login/naver";
  }
  kakaoLogin() {
    window.location.href = "https://cotex.hyunwoo.dev/api/auth/user/login/kakao";
  }
  githubLogin() {
    window.location.href = "https://cotex.hyunwoo.dev/api/auth/user/login/github";
  }

  async submit() {
    if (this.id && this.password) {
      if (!this.isRegister) {
        let res = await this.$store.dispatch("LOGIN", {
          userID: this.id,
          password: this.password,
        });
        if (res) {
          this.$router.replace("/");
        } else {
          alert("계정 정보가 일치하지 않습니다.");
        }
      } else {
        let res = await this.$store.dispatch("REGISTER", {
          userID: this.id,
          password: this.password,
        });
        if (res) {
          this.$router.replace("/");
        } else {
          alert("회원가입에 실패하였습니다.");
        }
      }
    }
  }
}
</script>

<style lang="scss">
.login {
  display: flex;
  width: 100vw;
  height: 100vh;

  background-color: $primary-color;

  text-align: left;

  hr {
    border: 1px solid #eeeeee;
    width: 50%;
    margin: 40px auto;
  }
  .login__content {
    flex: 1;
    overflow: hidden;

    display: flex;
    justify-content: center;
    align-items: center;

    .login__content__textbox {
      position: absolute;
      top: 10%;
      color: white;

      h1 {
        font-size: 50px;
        letter-spacing: 1.25px;
        text-align: center;
      }
      p {
        font-size: 20px;
        line-height: 1.9;
        letter-spacing: 0.5px;
        text-align: center;
      }
    }
    .image {
      width: 100%;
    }
  }
  .login__form {
    padding: 40px;

    background-color: white;
    width: 600px;

    display: flex;
    flex-direction: column;
    .login__form__header {
      h2 {
        font-size: 24px;
      }
      p {
        font-size: 14px;
      }
      .logo {
        display: inline-block;
        margin-bottom: 10px;
      }
    }
    .login__form__social {
      display: flex;
      flex-direction: column;
      h2 {
        text-align: center;
        margin: 20px 0;
        font-size: 16px;
      }
      .button {
        margin-bottom: 15px;
        padding: 8px;
        display: flex;
        justify-content: center;
        align-items: center;
        img,
        .iconify {
          margin-right: 7px;
        }
        &.login__form__social__naver {
          background-color: #1ec800;
        }
        &.login__form__social__kakao {
          background-color: #ffe812;
          color: $text-color;
        }
        &.login__form__social__github {
          background-color: #ffffff;
          border: 1px solid #eeeeee;
          color: $text-color;
          .github {
            font-size: 24px;
          }
        }
      }
    }
    .login__form__loginbox {
    }
    .login__from__submitBtn {
      margin-top: 40px;
      width: 100%;
      font-size: 14px;
    }
    .login__form__first {
      flex: 1;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 10px;
      b {
        margin-left: 5px;
        cursor: pointer;
      }
    }
  }
  .inputbox {
    .login__label {
      color: $secondary-color;
      font-weight: bold;

      margin: 20px 0 7px 0;
    }
    input {
      font-size: 14px;
    }
  }
}
</style>
