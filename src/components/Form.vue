<template>
  <div class="auth" ref="auth">
    <form action="" class="auth__form form">
      <label class="form__label" for="name"> Логин </label>
      <input class="form__field"
        :class="{'form__field--error' : validation.hasError('name')}"
        type="text"
        name="name"
        id="name"
        ref="input1"
        v-model="name"
        placeholder="Логин">
      <label class="form__label" for="pass"> Пароль </label>
      <input
        class="form__field"
        :class="{'form__field--error' : validation.hasError('pass')}"
        type="password"
        name="pass"
        id="pass"
        ref="input2"
        v-model="pass"
        placeholder="Пароль">
      <div class="form__footer">
        <transition name="fade">
          <button v-if="!status.inProcess" class="form__button" type="submit" @click.prevent="authorise"> Вход </button>
        </transition>
        <transition name="fade">
          <div v-if="status.inProcess" class="form__preloader">
            <div class="sk-rotating-plane"></div>
          </div>
        </transition>
      </div>
      
    </form>
    <transition name="fade">
      <div v-if="show || validation.firstError('name') || validation.firstError('pass')" class="auth__message">
        <span class="auth__validate"> {{ validation.firstError('name') || validation.firstError('pass') }} </span>
        <span v-if="status.error" class="auth__error"> {{ messages.error }} </span>
        <span v-if="status.success" class="auth__success"> {{ messages.success }} </span>
      </div>
    </transition>
  </div>
</template>

<script>

import { Validator } from "simple-vue-validator";

export default {
  name: 'Form',
  data: () => {
    return {
      name: '',
      pass: '',
      messages: {
        success: 'Успешная авторизация',
        error: 'Ошибка авторизации'
      },
      status: {
        success: false,
        error: false,
        inProcess: false
      },
      admin: {
        name: 'test',
        pass: 'test'
      },
      show: false,
      sendDelay: 5000,
      redirectDelay: 3000
    }
  },
  mixins: [require("simple-vue-validator").mixin],
  validators: {
      name: value => {
          return Validator.value(value).required("Логин не может быть пустым");
      },
      pass: value => {
          return Validator.value(value).required("Пароль не может быть пустым");
      },
  },
  methods: {
    authorise: function() {
      this.$validate().then(success => {
        this.resetStatus();
        if (!success) return;
        this.status.inProcess = true;
        this.$refs.input1.disabled = true;
        this.$refs.input2.disabled = true;
        new Promise( (resolve) => {
          setTimeout(() => {
            resolve();
          }, this.sendDelay);})
          .then(() => {
            if(this.checkPass(this.name, this.pass)) {
              setTimeout(() => {
                this.$refs.auth.classList.add('auth--loaded');
                this.$emit('loaded');
              }, this.redirectDelay);
            } else {
              this.status.inProcess = false;
              this.$refs.input1.disabled = false;
              this.$refs.input2.disabled = false;
              this.name = '';
              this.pass = '';
            }
            this.validation.reset();
        });
      });
    },
    checkPass: function (name, password) {
      if (name !== this.admin.name || password !== this.admin.pass) {
        this.status.error = true;
        this.show = true;
        return false
        }
      if (name == this.admin.name && password == this.admin.pass) {
        this.status.success = true;
        this.show = true;
        return true
        }
    },
    resetStatus: function () {
      this.status.error = false;
      this.status.false = false;
      this.show = false;
    }
  }
}
</script>

<style scoped lang="scss">

.auth {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.auth--loaded {
  position: absolute;
  transition: 0.5s;
  z-index: -5;
}

.auth__message {
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 2px 2px 10px 0 gray;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 2px 2px 10px 0 gray;
}

.form__field {
  width: 150px;
  border-radius: 10px;
  border: 1px solid gray;
  padding: 3px 10px;

  &--error {
    background-color: #f8a5a5;
    border: 1px solid red;
  }
}

.form__label {
  text-align: left;
  padding: 10px 0 ;
  display: block;
  
}

.form__footer {
  margin-top: 20px;
  position: relative;
  height: 46px;
  width: 100%;
}

.form__button {
  border-radius: 10px;
  background-color: lightgray;
  border: 1px solid gray;
  padding: 3px 10px;
  cursor: pointer;
  width: 150px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  &:hover, &:focus {
    background-color: gray;
    border: 1px solid #fff;
    color: #fff;
  }
}

.form__preloader {
  width: 20px;
  height: 20px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to  {
  opacity: 0;
}

.sk-rotating-plane {
  width: 100%;
  height: 100%;
  margin: auto;
  background-color: gray;
  animation: sk-rotating-plane 1.2s infinite ease-in-out;
}

@keyframes sk-rotating-plane {
  0% {
    transform: perspective(120px) rotateX(0deg) rotateY(0deg);
  }
  50% {
    transform: perspective(120px) rotateX(-180.1deg) rotateY(0deg);
  }
  100% {
    transform: perspective(120px) rotateX(-180deg) rotateY(-179.9deg);
  }
}

</style>
