<template>
  <div id="app">
    <div v-if="!thanks" class="container">
      <div class="top">
        <h1 class="title">Регистрация</h1>
      </div>

      <Separator />

      <div class="midle">
        <h3 class="form-title">Заполните Ваши данные</h3>
        <!--Дабы не грузить такое небольшое app библиотекой vuex, перекинем все через props-->
        <!--Сначала кажется, что все запутано. И вам это не кажется. Но в целом, если присмотреться, то жить можно-->
        <Form :usernameError="usernameError" :emailError="emailError" :roleError="roleError"
          :passwordError="passwordError" :passwordRepeatError="passwordRepeatError" @updateUsername="updateUsername"
          @updateEmail="updateEmail" @updateRole="updateRole" @updatePassword="updatePassword"
          @updatePassword_repeat="updatePassword_repeat" @updatePasswordRepeatError="updatePasswordRepeatError" />
      </div>

      <Separator />

      <div class="bottom">
        <!--Ну, тут так же обойдемся без vuex-->
        <Switcher @updateSwitch="updateSwitch" />
        <Check :username="username" :email="email" :role="role" :password="password" :password_repeat="password_repeat"
          :public="this.public" :error="passwordRepeatError" @updateUsernameError="updateUsernameError"
          @updateEmailError="updateEmailError" @updateRoleError="updateRoleError"
          @updatePasswordError="updatePasswordError" @updateThanks="updateThanks" />
      </div>
    </div>
    <div v-else class="container">
      <p class="thanks">Регистрация успешно завершена</p>
      <i class="cross" @click="closeTanks"></i>
    </div>
  </div>
</template>

<script>
import Separator from '@/components/separator-app.vue';
import Form from '@/components/form-app.vue';
import Switcher from '@/components/switcher-app.vue';
import Check from '@/components/check-app.vue';

export default {
  name: 'App',
  components: {
    Separator,
    Form,
    Switcher,
    Check
  },

  data: () => ({
    username: '',
    email: '',
    role: null,
    password: '',
    password_repeat: '',
    public: false,
    //Ошибки
    usernameError: false,
    emailError: false,
    roleError: false,
    passwordError: false,
    passwordRepeatError: false,
    //Успешная отправка
    thanks: false
  }),

  methods: {
    //Передаем имя в check
    updateUsername(value) {
      this.username = value;
    },
    //получаем ошибку имени и передаем ее в form
    updateUsernameError(value) {
      this.usernameError = value
    },
    //Передаем email в check
    updateEmail(value) {
      this.email = value
    },
    //получаем ошибку email и передаем ее в form
    updateEmailError(value) {
      this.emailError = value
    },
    //Передаем должность в check
    updateRole(value) {
      this.role = value
    },
    //получаем ошибку должности и передаем ее в form
    updateRoleError(value) {
      this.roleError = value
    },
    //Передаем password в check
    updatePassword(value) {
      this.password = value
    },
    //получаем ошибку password и передаем ее в form
    updatePasswordError(value) {
      this.passwordError = value
    },
    //Передаем password_repeat в check
    updatePassword_repeat(value) {
      this.password_repeat = value
    },
    //Получаем ответ проверки совпадения паролей
    updatePasswordRepeatError(value) {
      this.passwordRepeatError = value
    },
    //Передаем public в check
    updateSwitch(value) {
      this.public = value
    },
    //Выводим "Спасибо при успешной отправки формы"
    updateThanks(value) {
      this.thanks = value
    },
    //Закрываем спасибо
    closeTanks() {
      this.thanks = false
      this.username = ''
      this.email = ''
      this.role = null
      this.password = ''
      this.password_repeat = ''
      this.public = false

      this.usernameError = false
      this.emailError = false
      this.roleError = false
      this.passwordError = false
      this.passwordRepeatError = false
    }
  }
}
</script>

<style>
#app {
  font-family: Montserrat, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
}

.container {
  position: relative;
  margin: 0 auto;
  max-width: 960px;
  border-radius: 15px;
  border: 1px solid #D9D9D9;
}

.top {
  padding: 17px 15px 0px 31px;
}

.title,
.thanks {
  margin-top: 0;
  margin-bottom: 17px;
  font-size: 19px;
  font-weight: 700;
  line-height: 27px;
  letter-spacing: -0.0035em;
  text-align: left;
}

.thanks {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

.midle {
  padding: 17px 15px 30px 31px;
}

.form-title {
  margin-top: 0;
  margin-bottom: 34px;
  font-size: 16px;
  font-weight: 500;
  line-height: 19px;
  letter-spacing: 0em;
  text-align: left;
}

.bottom {
  padding: 23px 15px 66px 31px;
}

.cross {
  position: absolute;
  width: 10px;
  height: 10px;
  top: 10px;
  right: 10px;
  cursor: pointer;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  background-image: url("data:image/svg+xml,%3Csvg fill='%23000000' height='800px' width='800px' version='1.1' id='Capa_1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' viewBox='0 0 490 490' xml:space='preserve'%3E%3Cpolygon points='456.851,0 245,212.564 33.149,0 0.708,32.337 212.669,245.004 0.708,457.678 33.149,490 245,277.443 456.851,490 489.292,457.678 277.331,245.004 489.292,32.337 '/%3E%3C/svg%3E");
}


@media(max-width:610px) {

  .top,
  .midle,
  .bottom {
    padding-left: 15px;
  }

  .bottom {
    padding-bottom: 30px;
  }
}
</style>
