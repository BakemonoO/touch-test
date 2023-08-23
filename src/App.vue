<template>
  <div>
    <transition name="modal">
      <div
        v-if="visible"
        class="draggable-block"
        ref="draggableBlock"
        @touchstart="handleTouchStart"
        @touchmove="handleTouchMove"
        @touchend="handleTouchEnd"
        :style="{ bottom: blockBottom + '%' }"
      >
        модалОчка
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      blockBottom: 0,
      startTouchY: 0,
      visible: true
    };
  },
  methods: {
    handleTouchStart(event) {
      event.preventDefault();
      this.startTouchY = event.touches[0].clientY;
    },
    handleTouchMove(event) {
      const deltaY = event.touches[0].clientY - this.startTouchY;
      const distanceInPercent = (deltaY / window.innerHeight) * 100;
      if (distanceInPercent > 0) {
        this.blockBottom -= distanceInPercent; // Изменили знак на минус
        this.startTouchY = event.touches[0].clientY;
      }
    },
    handleTouchEnd() {
      if (this.blockBottom < 0) {
        this.visible = false;
      }
      // Ваш код для обработки окончания перемещения на сенсорном устройстве
    }
  }
};
</script>

<style scoped>
.draggable-block {
  position: absolute;
  width: 100%;
  height: 300px;
  bottom: 10%;
  background-color: #ccc;
  cursor: grab;
  transition: transform 0.1s ease; /* Добавляем плавную анимацию */
}

.modal-enter-active,
.modal-leave-active {
  transition: all 1s ease;
}

.modal-enter-from,
.modal-leave-to {
  transform: translate(0, 100%);
}
</style>
