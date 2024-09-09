<template>
  <div id="app" @mousemove="updateCursorPosition" @click="handleMouseClick">
    <!-- Background container -->
    <div class="background"></div>

    <!-- Sprite Animator and Obstacles -->
    <SpriteAnimator
      :frameWidth="24"
      :frameHeight="24"
      :animationSpeed="100"
      :spriteSheet="spriteSheetPath"
      :bowSpriteSheet="bowSpriteSheetPath"
      :arrowImage="arrowImagePath"
      :scale="1.5"
      :obstacles="obstacles"
      :mouseX="mouseX"
      :mouseY="mouseY"
      :shoot="shoot"
    />
    <div
      v-for="(obstacle, index) in obstacles"
      :key="index"
      :style="getObstacleStyle(obstacle)"
      class="obstacle"
    ></div>
  </div>
</template>

<script>
import SpriteAnimator from '@/components/SpriteAnimator.vue'
import spriteSheetPath from '@/assets/Character/Girl-Sheet.png'
import bowSpriteSheetPath from '@/assets/Character/Bow.png'
import arrowImagePath from '@/assets/Character/Arrow.png'

export default {
  name: 'App',
  components: {
    SpriteAnimator
  },
  data() {
    return {
      spriteSheetPath,
      bowSpriteSheetPath,
      arrowImagePath,
      obstacles: [
        { x: 100, y: 100, width: 50, height: 50 },
        { x: 200, y: 150, width: 50, height: 50 },
        { x: 150, y: 300, width: 50, height: 50 }
      ],
      mouseX: 0,
      mouseY: 0,
      shoot: false // To trigger shooting
    }
  },
  watch: {
    shoot(newVal) {
      if (newVal) {
        this.$nextTick(() => {
          this.shoot = false // 다음 틱에서 shoot 상태 초기화
        })
      }
    }
  },
  methods: {
    getObstacleStyle(obstacle) {
      return {
        position: 'absolute',
        left: `${obstacle.x}px`,
        top: `${obstacle.y}px`,
        width: `${obstacle.width}px`,
        height: `${obstacle.height}px`
      }
    },
    updateCursorPosition(event) {
      this.mouseX = event.clientX
      this.mouseY = event.clientY
    },
    handleMouseClick() {
      if (!this.shoot) {
        this.shoot = true
      }
    }
  }
}
</script>

<style>
#app {
  position: relative;
  width: 100%;
  height: 96vh;
  overflow: hidden;
  cursor: crosshair; /* Cursor change to indicate aiming */
}

/* Background styling */
.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/Map/TX Tileset Grass.png');
  background-repeat: repeat;
  z-index: -1; /* Ensure background is behind other elements */
}

.obstacle {
  background-image: url('@/assets/Map/TX Plant.png'), url('@/assets/Map/TX Shadow Plant.png');
  background-repeat: no-repeat;
  background-position: -215px -180px;
}
</style>
