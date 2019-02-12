<template>
  <div v-if="login" class="hello">
    <div class="hello__container">
      <h1 class="hello__header">Добро пожаловать.</h1>
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
  margin: 0 10px;
}

.hello__header {
  @media screen and (max-width: 500px) {
    font-size: 20px;
  }
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
