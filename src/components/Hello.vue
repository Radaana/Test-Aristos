<template>
  <div v-if="login" class="hello">
    <div class="hello__container">
      <h1>Добро пожаловать.</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hello',
  props: {
    login: Boolean
  }
}
</script>

<style scoped lang="scss">

.hello {
  display: flex;
  align-items: center;
  justify-content: center;
}

.hello__container {
  background-color: #fff;
  border-radius: 10px;
  padding: 20px 30px;
  box-shadow: 2px 2px 10px 0 gray;
  animation: fade 1.0s ease-in-out;
}

@keyframes fade {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

</style>
