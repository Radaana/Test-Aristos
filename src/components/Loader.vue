<template>
  <div class="loader" ref="loader">
    <div class="loader__pic">
      <div class="sk-double-bounce">
        <div class="sk-child sk-double-bounce-1"></div>
        <div class="sk-child sk-double-bounce-2"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Loader',
  data: () => {
    return {
      delayTime: 5000,
      fadeTime: 400,
    }
  },
  mounted: function () {
    this.$nextTick(function () {
      new Promise( (resolve) => {
        setTimeout(() => {
          this.$refs.loader.classList.add('loader--fade')
          resolve();
        }, this.delayTime);})
      .then(() => {
        setTimeout(() => {
          this.$refs.loader.classList.add('loader--hide')
        }, this.fadeTime);
      });
    })
  }
}
</script>

<style scoped lang="scss">
.loader {
  background: linear-gradient(to bottom, gray, lightblue);
  background-size: cover;
  overflow: hidden;
  position: fixed;
  left: 0;
  top: 0;
  right:0;
  bottom:0;
  z-index: 100000;
  transition: opacity 0.4s;
  opacity: 1;
}

.loader--fade {
  opacity: 0;
}

.loader--hide {
  height: 1px;
  width: 1px;
  right: initial;
  bottom: initial;
  z-index: -100000;
}

.loader__pic {
  width: 100px;
  height: 100px;
  position: absolute;
  top: calc(50% - 50px);
  left: calc(50% - 50px);
}

.sk-double-bounce {
  width: 100px;
  height: 100px;
  position: relative;

  .sk-child {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #fff;
    opacity: 0.6;
    position: absolute;
    top: 0;
    left: 0;
    animation: sk-double-bounce 2.0s infinite ease-in-out;
  }

  .sk-double-bounce-2 {
    animation-delay: -1.0s;
  }
}

@keyframes sk-double-bounce {
  0%, 100% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.0);
  }
}

</style>