<template>
  <div class="up">
    <button class="btn infinity-button" @click="Up">
      {{this.emonji}}
    </button>
  </div>
</template>

<script>
export default {
  name: 'ButtonUp',
  props: {
    emonji: String
  },
  methods: {
    Up(){
      function smoothScroll(target, duration) {
            let getHash = document.querySelector(target)
            let targetPosition = getHash.getBoundingClientRect().top
            let startPosition = window.pageYOffset;
            let distance = targetPosition
            let startTime = null
            function animation(currentTime){
                if(startTime === null) startTime = currentTime
                let timeElapsed = currentTime - startTime
                let run = ease(timeElapsed, startPosition, distance, duration)
                window.scrollTo(0, run)
                if(timeElapsed < duration) requestAnimationFrame (animation)
            }
            function ease(t, b, c, d) {
                t /= d / 2
                if(t < 1) return c / 2 * t * t + b
                t--
                return -c / 2 * (t * (t - 2) - 1) + b
            }
            requestAnimationFrame(animation)
        }
        //CALL FUNCTION
        smoothScroll(`#infinity_content`, 1500)
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .infinity-button {
    position: fixed;
    bottom: 20px;
    right: 30px;
    font-size: 40px;
    transform: rotate(270deg);
    background: #0083B0;
    padding: 10px;
    border-radius: 10px;
    color: #fff;
  }
  .infinity-button:hover{
    background: #005C7A;
  }
</style>
